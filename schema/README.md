# Schema Development Structure

This directory contains the TypeDB ontological schema and educational visualizations for ISA-95 / DIN 62264-2 manufacturing standards.

## Folder Organization

### `/ontology/`
Contains TypeDB schema files (.tql) implementing the ISA-95 concepts:
- Entity definitions
- Relationship definitions
- Rules and constraints
- Type hierarchies

### `/visualizations/`
Contains DrawIO diagram files (.drawio) providing educational visualizations:
- **NOT UML diagrams** - focused on concepts and knowledge
- Shows attributes, values, descriptions from the specification
- Helps users understand the meaning of each concept
- Organized by DIN 62264 document structure

### `/documents/`
Contains notes and references from DIN 62264-2 documents:
- Chapter-by-chapter documentation
- Reference material from standard
- Implementation notes

## File Naming Convention

Files are named with their document chapter number:
- `01_*.drawio` - Chapter 1 concepts
- `02_*.drawio` - Chapter 2 concepts (Hierarchical Scope)
- `03_*.drawio` - Chapter 3 concepts
- etc.

Same pattern applied to ontology files.

## Approach: Parallel Development

For each DIN 62264 concept:

1. **Study the specification** - understand the concept thoroughly
2. **Create visualization** - DrawIO file explaining:
   - What the concept means
   - Key attributes and their descriptions
   - Possible values
   - Relationships to other concepts
   - Practical examples
3. **Create ontology** - TypeDB schema file implementing the concept
4. **Document notes** - reference material and implementation decisions

## Current Progress

- [x] Document 2: Hierarchical Scope
  - [x] Visualization: `02_hierarchical_scope.drawio`
  - [ ] Ontology: `02_hierarchical_scope.tql`
  - [ ] Documentation notes

## How to Use

1. **For Understanding**: Open the `.drawio` files in DrawIO to visualize concepts
2. **For Implementation**: Reference the `.tql` files for TypeDB schema
3. **For Reference**: Check `/references/DIN62264/` for official documentation
