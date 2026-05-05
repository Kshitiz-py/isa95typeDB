# ISA-95 / DIN 62264-2 Development Checklist

This checklist is the authoritative base for tracking our implementation of DIN 62264-2 in DrawIO diagrams and TypeDB ontology.
Use it first for all standard objects and sections. If we add items that are not in the standard, record them in the **Non-standard additions** section below and highlight them clearly.

> Non-standard additions should be tracked with a visible label like `NON-STANDARD` or using orange text to distinguish them from standard checklist items.
>
> Example: `<span style="color:orange">NON-STANDARD:</span> Additional implementation-specific object`

## Section 4.7 — Hierarchy Scope

- [x] Confirm that `Hierarchy Scope` is described as an attribute used by many objects
- [x] Confirm that `Hierarchy Scope` is optional if the exchange mechanism already provides context
- [x] Confirm the examples for scope values:
  - [x] Single Site identifier (e.g. `WEST-END`)
  - [x] Site/Area path (e.g. `WEST-END/HOLDING-AREA`)
  - [x] Work Center path (e.g. `WEST-END/HOLDING-AREA/CHIPPING-BIN #1`)
  - [x] Work Center only when context is known (e.g. `CHIPPING-BIN #1`)
  - [x] Complete hierarchy example (Enterprise, Site, Area, Work Center)
- [x] Confirm that the hierarchy scope attribute may be modeled using a Hierarchy Scope object with Table 3 attributes
- [x] Confirm Table 3 attributes:
  - [x] Equipment ID
  - [x] Equipment Element Level
- [x] Confirm example values for Equipment ID and Equipment Element Level
- [ ] Identify which objects in the ontology use `Hierarchy Scope` (e.g. Production Performance, Production Capability, Maintenance, Quality, Inventory)
- [x] Add a visual diagram file for `Hierarchy Scope` showing:
  - [x] Concept definition
  - [x] Optionality and usage
  - [x] Example values and paths
  - [x] Table 3 attributes
  - [x] Relationship to consuming objects
- [x] Add TypeDB ontology for `Hierarchy Scope`
- [ ] Add example TypeQL pattern for an object linking to `Hierarchy Scope`

## Section 5 — Common Object Models

### 5.1 Personnel Information

- [x] Read section 5.1 and identify all personnel-related objects and attributes
- [x] Create DrawIO diagram for Personnel model (Figure 5)
- [x] Create TypeDB ontology for:
  - [x] Person
  - [x] Personnel Class
  - [x] Person Property
  - [x] Personnel Class Property
  - [x] Qualification Test Specification
  - [x] Qualification Test Result
- [x] Add documentation notes for personnel information

### 5.2 Equipment Information

- [ ] Read section 5.2 and identify all equipment-related objects and attributes
- [ ] Create DrawIO diagram for Equipment model (Figure 6)
- [ ] Create TypeDB ontology for:
  - [ ] Equipment
  - [ ] Equipment Class
  - [ ] Equipment Property
  - [ ] Equipment Class Property
  - [ ] Equipment Capability Test Specification
  - [ ] Equipment Capability Test Result
- [ ] Add documentation notes for equipment information

### 5.3 Material Information

- [ ] Read section 5.3 and identify all material-related objects and attributes
- [ ] Create DrawIO diagram for Material model (Figure 7)
- [ ] Create TypeDB ontology for:
  - [ ] Material Definition
  - [ ] Material Class
  - [ ] Material Lot
  - [ ] Material Sublot
  - [ ] Material Test Specification
  - [ ] Material Test Result
  - [ ] Material Property
  - [ ] Material Class Property
- [ ] Add documentation notes for material information

### 5.4 Process Segment Information

- [ ] Read section 5.4 and identify all process segment-related objects and attributes
- [ ] Create DrawIO diagram for Process Segment model (Figure 8)
- [ ] Create TypeDB ontology for:
  - [ ] Process Segment
  - [ ] Personnel Segment Specification
  - [ ] Equipment Segment Specification
  - [ ] Material Segment Specification
  - [ ] Physical Asset Segment Specification
  - [ ] Segment Parameter
  - [ ] Segment Property
- [ ] Add documentation notes for process segment information

### 5.5 Operations Definition Information

- [ ] Read section 5.5 and identify all operations definition-related objects and attributes
- [ ] Create DrawIO diagram for Operations Definition model (Figure 9)
- [ ] Create TypeDB ontology for:
  - [ ] Operations Definition
  - [ ] Operations Material Bill
  - [ ] Operations Request
  - [ ] Job Order
  - [ ] Job Response
- [ ] Add documentation notes for operations definition information

### 5.6 Operations Schedule Information

- [ ] Read section 5.6 and identify all operations schedule-related objects and attributes
- [ ] Create DrawIO diagram for Operations Schedule model (Figure 10)
- [ ] Create TypeDB ontology for:
  - [ ] Operations Schedule
  - [ ] Production Schedule
  - [ ] Maintenance Schedule
  - [ ] Quality Test Schedule
  - [ ] Inventory Schedule
- [ ] Add documentation notes for operations schedule information

### 5.7 Operations Performance Information

- [ ] Read section 5.7 and identify all operations performance-related objects and attributes
- [ ] Create DrawIO diagram for Operations Performance model (Figure 11)
- [ ] Create TypeDB ontology for:
  - [ ] Operations Performance
  - [ ] Production Performance
  - [ ] Maintenance Performance
  - [ ] Quality Test Performance
  - [ ] Inventory Performance
- [ ] Add documentation notes for operations performance information

### 5.8 Operations Capability Information

- [ ] Read section 5.8 and identify all operations capability-related objects and attributes
- [ ] Create DrawIO diagram for Operations Capability model (Figure 12)
- [ ] Create TypeDB ontology for:
  - [ ] Operations Capability
  - [ ] Production Capability
  - [ ] Maintenance Capability
  - [ ] Quality Test Capability
  - [ ] Inventory Capability
- [ ] Add documentation notes for operations capability information

## Section 6 — Production Operations Management

- [ ] Read section 6 and identify production-specific objects and attributes
- [ ] Create DrawIO diagrams for production models
- [ ] Create TypeDB ontology for production-specific entities
- [ ] Add documentation notes for production operations

## Section 7 — Maintenance Operations Management

- [ ] Read section 7 and identify maintenance-specific objects and attributes
- [ ] Create DrawIO diagrams for maintenance models
- [ ] Create TypeDB ontology for maintenance-specific entities
- [ ] Add documentation notes for maintenance operations

## Section 8 — Quality Operations Management

- [ ] Read section 8 and identify quality-specific objects and attributes
- [ ] Create DrawIO diagrams for quality models
- [ ] Create TypeDB ontology for quality-specific entities
- [ ] Add documentation notes for quality operations

## Section 9 — Inventory Operations Management

- [ ] Read section 9 and identify inventory-specific objects and attributes
- [ ] Create DrawIO diagrams for inventory models
- [ ] Create TypeDB ontology for inventory-specific entities
- [ ] Add documentation notes for inventory operations

## Non-standard additions

Use this section to record implementation items that are not directly defined in DIN 62264-2.
Mark them clearly as `NON-STANDARD` and explain why they were added.

- [ ] <span style="color:orange">NON-STANDARD:</span> 

## Diagram and Ontology Workflow

- [x] Create folder structure for reference, visualizations, ontology, and documents
- [x] Create diagram files using `.drawio` format, not UML, focusing on concept explanation and attribute definitions
- [x] Create TypeDB schema files in `schema/ontology/`
- [x] Keep a roadmap of chapter-based progress in `schema/README.md`
- [ ] Add documentation notes for each section in `schema/documents/`
- [ ] Validate diagrams open correctly in DrawIO
- [ ] Validate TypeDB schema syntax and semantics for each section
- [ ] Review and update the checklist as new sections are covered

## Future Section Checklist Template

For each new DIN 62264 document section:

- [ ] Read the section and identify the concept name
- [ ] Capture the definition and how it is used
- [ ] Identify all attributes and their examples
- [ ] Identify if the concept is modeled as an attribute, object, property, or relation
- [ ] Create a visualization that explains the concept clearly for users
- [ ] Create corresponding TypeDB ontology schema
- [ ] Add source references and implementation notes
- [ ] Verify the model against the specification# ISA-95 / DIN 62264-2 Development Checklist

This checklist is for building the ISA-95/DIN 62264-2 visualizations and TypeDB ontology.
Mark each item as completed as you progress.

## Section 4.7 — Hierarchy Scope

- [x] Confirm that `Hierarchy Scope` is described as an attribute used by many objects
- [x] Confirm that `Hierarchy Scope` is optional if the exchange mechanism already provides context
- [x] Confirm the examples for scope values:
  - [x] Single Site identifier (e.g. `WEST-END`)
  - [x] Site/Area path (e.g. `WEST-END/HOLDING-AREA`)
  - [x] Work Center path (e.g. `WEST-END/HOLDING-AREA/CHIPPING-BIN #1`)
  - [x] Work Center only when context is known (e.g. `CHIPPING-BIN #1`)
  - [x] Complete hierarchy example (Enterprise, Site, Area, Work Center)
- [x] Confirm that the hierarchy scope attribute may be modeled using a Hierarchy Scope object with Table 3 attributes
- [x] Confirm Table 3 attributes:
  - [x] Equipment ID
  - [x] Equipment Element Level
- [x] Confirm example values for Equipment ID and Equipment Element Level
- [ ] Identify which objects in the ontology use `Hierarchy Scope` (e.g. Production Performance, Production Capability, Maintenance, Quality, Inventory)
- [ ] Add a visual diagram file for `Hierarchy Scope` showing:
  - [ ] Concept definition
  - [ ] Optionality and usage
  - [ ] Example values and paths
  - [ ] Table 3 attributes
  - [ ] Relationship to consuming objects
- [x] Add TypeDB ontology for `Hierarchy Scope`
- [ ] Add example TypeQL pattern for an object linking to `Hierarchy Scope`

## Diagram and Ontology Workflow

- [x] Create folder structure for reference, visualizations, ontology, and documents
- [x] Create diagram files using `.drawio` format, not UML, focusing on concept explanation and attribute definitions
- [x] Create TypeDB schema files in `schema/ontology/`
- [x] Keep a roadmap of chapter-based progress in `schema/README.md`
- [ ] Add documentation notes for each section in `schema/documents/`
- [ ] Validate diagrams open correctly in DrawIO
- [ ] Validate TypeDB schema syntax and semantics for each section
- [ ] Review and update the checklist as new sections are covered

## Future Section Checklist Template

For each new DIN 62264 document section:

- [ ] Read the section and identify the concept name
- [ ] Capture the definition and how it is used
- [ ] Identify all attributes and their examples
- [ ] Identify if the concept is modeled as an attribute, object, property, or relation
- [ ] Create a visualization that explains the concept clearly for users
- [ ] Create corresponding TypeDB ontology schema
- [ ] Add source references and implementation notes
- [ ] Verify the model against the specification
