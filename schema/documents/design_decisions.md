# TypeDB Implementation Design Decisions & Findings

This document captures key design decisions, implementation details, and architectural findings for the IEC 62264 TypeDB ontology. It serves as a reference for application developers and a basis for the final report.

## 1. Property Modeling (Entities vs. Attributes)

In this implementation, properties (e.g., `person-property`, `personnel-class-property`) are modeled as distinct **entities** rather than simple string or value attributes directly attached to a parent.

**Finding & Benefit:**
This enables highly granular, independent property mapping. 
* A `personnel-class` might define expected properties like "name" and "engineering class".
* A `person` might possess those same properties, plus additional, independent ones like "working experience".
* By modeling properties as entities, we can explicitly map the "name" and "engineering class" person properties to their corresponding personnel class properties using the `corresponds-to-personnel-class-property` relation.
* The "working experience" `person-property` entity remains unmapped, living independently under the `person` entity without being forced into the class definition.

## 2. Composition and Cascading Deletes

The IEC 62264 standard defines strict composition relationships (e.g., a Person *owns* Person Properties; a Personnel Class *owns* Personnel Class Properties). 

**Implementation Challenge:**
TypeDB does not natively support cascading deletes at the database level.

**Design Decision:**
* Cascading deletes **must be enforced at the application or API layer**.
* When a parent entity (such as a `person` or `personnel-class`) is deleted, the application logic must query for and explicitly delete all owned sub-entities (e.g., `person-property`, `personnel-class-property`) and their corresponding relations.
* This prevents owned properties and associations from becoming "orphaned" or left hanging in the graph without an owner.

## 3. Role-Based Resource Cardinality and Mixed Levels

The IEC 62264 standard establishes Role Based Models for various manufacturing resources (Personnel, Equipment, Physical Assets, Material). A core concept of these models is that they emphasize functional roles rather than strict physical constraints.

**Implementation Details:**
* The relations connecting specific resource instances to their classes (e.g., `belongs-to-equipment-class`, `belongs-to-personnel-class`) are implemented to enable a many-to-many cardinality.
* This explicitly aligns with IEC 62264-2 principles (e.g., Section 5.2.2 for Equipment dictates: *"Any piece of equipment may be a member of zero or more equipment classes"*; the same logic implicitly or explicitly applies across other resource types like Personnel, where a person can belong to multiple personnel classes).

**Design Implication (Multi-Level Functional Roles):**
For resource types that possess hierarchical "levels" (like `equipment-level` for `equipment` and `equipment-class`), mapping a single instance to multiple classes with **different** levels signifies **Multi-Level Functional Roles**.

Because IEC 62264 defines levels hierarchically based on *logical roles* in manufacturing models, a resource's level can shift depending on the operational context. For example:
* A mobile test system or packaged skid might logically function as an entire independent "Unit" (a higher equipment level) in one process flow.
* That exact same physical equipment might act as a lower-level "Work Cell" when integrated as a sub-component within a larger manufacturing system for a different process.

Associating a resource instance with multiple classes—potentially of varying operational levels—accurately captures this context-dependent, multi-scalar behavior in the TypeDB graph across the entire IEC 62264 standard framework.

## 4. Physical Asset Constraints and Mapping

While the *Role-Based Equipment Model* supports many-to-many relationships (as an equipment role can logically belong to multiple classes), the **Physical Asset Model** represents actual, tangible items (often tracked by serial numbers or tags). 

**Design Implication (Physical Asset Class Cardinality):**
* According to IEC 62264-2 Section 5.3.4: *"Any physical asset shall be a member of one physical asset class."*
* Therefore, the relationship `belongs-to-physical-asset-class` has a stricter **0..1 (or exactly 1) cardinality constraint** from the perspective of the Physical Asset. 
* This constraint is documented in the ontology and visualization, but like cascading deletes, must be enforced at the application or API layer since TypeDB's open-world relational model does not natively enforce upper-bound cardinality limits out-of-the-box.

**Equipment Asset Mapping:**
* To bridge the gap between logical roles (Equipment) and tangible items (Physical Assets), the standard defines the **Equipment Asset Mapping** (Section 5.3.8).
* In our TypeDB implementation, this is modeled as the `equipment-asset-mapping` relation. It connects `mapped-equipment` (played by `equipment`) and `mapped-physical-asset` (played by `physical-asset`), while owning `equipment-asset-mapping-start-time` and `equipment-asset-mapping-end-time` attributes to preserve the temporal history of asset utilization within logical roles.

## 5. Modular Ontologies and Schema Extension

To keep the TypeDB codebase manageable and aligned with the sections of IEC 62264-2, the ontology is split into multiple `.tql` files (e.g., `03_personnel_information.tql`, `04_role_based_equipment.tql`, `05_physical_asset_information.tql`).

**Design Implication (Cross-File Relations & Schema Extension):**
Because models frequently interact (e.g., the Physical Asset model maps to the Role-Based Equipment model), we utilize TypeDB's **Schema Extension** capabilities. 
* An entity is defined as a `sub entity` in its primary file (e.g., `equipment` is defined in file `04`).
* In subsequent files, that entity can be extended simply by referencing it. For example, in file `05`, we use `equipment plays mapped-equipment;` to give the previously defined equipment entity a new role for the `equipment-asset-mapping` relation.

**TypeQL Attribute Definition Policy:**
In the early stages of schema modeling, attributes (e.g., `person-id`, `description`) were assumed to be defined globally. However, to ensure each schema module is strictly self-contained, valid, and compileable out-of-the-box, **all attributes must be defined locally within the scope of their respective entities or relations.** 
* When creating a new entity or relation, developers must explicitly define all attributes it `owns` using `sub attribute, value <type>;` immediately preceding the entity/relation block. 
* This localized attribute definition pattern ensures that if a developer drops an isolated `05_...tql` file into an empty TypeDB instance, it fails only on cross-file dependencies (like `equipment`) rather than throwing hundreds of basic attribute typing errors.

**Strict Load Order Requirement:**
Because schemas are evaluated incrementally, **file load order is critical**. The numeric prefixes on the files (`02_`, `03_`, `04_`, `05_`) dictate the exact order they must be committed to the TypeDB database. If `05_physical_asset_information.tql` is loaded before `04_role_based_equipment.tql`, the compiler will fail because it attempts to extend an `equipment` entity that does not yet exist in the database graph.

## 6. Naming Conventions: Compositions vs. Associations

To ensure querying predictability and clear architectural semantics across the IEC 62264 models, strict naming conventions are enforced for relation verbs in the ontology.

**Design Implication (The `has-` Prefix Rule):**
* The prefix `has-` is **strictly reserved for Composition relationships**. 
* Any relation starting with `has-` (e.g., `has-personnel-class-property`, `has-equipment-sub-property`) implies an ownership lifecycle. If the parent entity is deleted, application-layer logic **must** cascade the deletion to the child property.
* For standard associations that do not imply strict lifecycle ownership, other descriptive verbs are used. For example, hierarchical nesting of tangible assets is defined using `made-up-of-` (e.g., `made-up-of-equipment`, `made-up-of-physical-asset`) to explicitly differentiate it from composition. Other common association verbs include `belongs-to-`, `corresponds-to-`, and `tested-by-`.
