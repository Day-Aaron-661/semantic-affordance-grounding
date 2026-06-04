# Homework 5: Ontology-based Semantic Grounding Report
**Group 14 | AI Capstone 2026**

## 1. Introduction and Repository Contents
This report documents the semantic grounding layer developed by Group 14. Our repository provides a knowledge representation framework that allows a robot to infer actionable knowledge—such as graspability and operability—from raw object observations. 
The repository includes:
* `ontology/group-ontology.ttl`: The core ontology containing our assertions and advanced logic.
* `ontology/inferred-results.ttl`: The materialized graph containing reasoned class assertions.
* `ontology/imports/course-affordance.ttl`: The provided baseline vocabulary.
* `queries/`: SPARQL scripts for validating inferences.
* `results/`: Execution screenshots and output logs.

## 2. Ontology Design & Rationale
Our design philosophy strictly adheres to separating object identities from their semantic capabilities (affordances) and dynamic contexts (task roles). A robot shouldn't just be hardcoded to know a "cup" is graspable; it must deduce this because the cup possesses a `GraspingAffordance`.

### 2.1 Namespace Policy
To prevent vocabulary collisions and respect the baseline structure, we utilized two namespaces:
* **Base (`cap:`)**: We imported and reused `https://hcis.io/ontology/aicapstone/2026/` for standardized concepts like `PhysicalObject`, `GraspingAffordance`, and property definitions (`hasTaskRole`, `hasPoseFrame`).
* **Group (`g14:`)**: We established `https://hcis.io/ontology/aicapstone/2026/group14/` for all custom instances (e.g., `g14:blueCup01`), newly introduced classes for our advanced task (e.g., `g14:Drawer`), and our custom reasoning properties.

### 2.2 Reused and Newly Introduced Terms (Advanced Task)
In addition to the entry-level tasks (Cup stacking, Cutlery, Blocks), we implemented an advanced **"Cookie Stowage Task"**. We modeled a scenario where a robot must identify cookie boxes and stow them into specific openable drawers.
* **Newly Introduced Classes**: `g14:Table`, `g14:Drawer`, `g14:CookieBox`.
* **Newly Introduced Affordance**: `g14:OpenableAffordance`.
* **Custom Object Property**: `g14:assignedToContainer` (used to semantically link a specific cookie box to its designated drawer target).

## 3. Reasoning Patterns and Key Axioms
Our ontology goes beyond static labeling by implementing three major inference rules using `owl:equivalentClass` and `owl:intersectionOf` coupled with existential restrictions.

1. **GraspableObject (Baseline)**: Inferred if a `PhysicalObject` has a `GraspingAffordance`.
2. **StackableObject (Advanced)**: Inferred if a `PhysicalObject` has a `StackabilityAffordance`. 
3. **OpenableObject (Advanced)**: Inferred if a `PhysicalObject` has a `g14:OpenableAffordance`.

By assigning these affordances at the class level (e.g., declaring that all `g14:Drawer` instances possess `ContainmentAffordance` and `OpenableAffordance`), the HermiT reasoner successfully propagates these properties down to the individuals (`drawer01`, `drawer02`), classifying them automatically without manual assertions.

## 4. Query Results and Verification
We validated our ontology using SPARQL queries in Protégé (via the Snap SPARQL Query plugin). 

### 4.1 Graspable Objects Verification
The query executed from `graspable_objects.rq` correctly retrieved all objects capable of being grasped. As expected, objects like `plate01`, `basket01`, and `drawer01` were intelligently filtered out by the reasoner because they lack the required affordance.

*(Please insert your `results/graspable_objects_output.txt` screenshot here)*

### 4.2 Advanced Task Verification
We ran `advanced_task.rq` to query for `g14:OpenableObject`. The reasoner perfectly isolated `drawer01`, `drawer02`, and `drawer03`, proving that our custom affordances and advanced classes were successfully integrated and inferable.

*(Please insert your advanced task screenshot here)*

## 5. Design Choices, Limitations, and Future Work
* **Design Choice**: We chose to expand the block collection task from one block to three (`block01` to `block03`) with distinct colors to better represent realistic clutter in a robot's perception pipeline.
* **Limitations**: Protégé's default SPARQL interface encountered a Java `NoClassDefFoundError` due to legacy library dependencies (Guava). We overcame this by switching to the `Snap SPARQL Query` view, which provided stable execution. Additionally, OWL reasoning is monotonic; if an object's affordance changes dynamically during a task (e.g., a cup breaks and is no longer stackable), static OWL cannot easily represent this state change without extensions.
* **Future Work**: For future integration into the Physical AI workflow, this ontology could be coupled with SHACL shapes to validate whether the perception pipeline is providing all required properties (e.g., ensuring every `TargetObject` has a `PoseFrame`) before the robot attempts a motion plan.

