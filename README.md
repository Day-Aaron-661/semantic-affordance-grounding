github link : https://github.com/Day-Aaron-661/semantic-affordance-grounding

# AI Capstone 2026 - Group 14: Ontology-based Semantic Grounding

## 1. Project Title and Group Members
* **Project**: Semantic Grounding and Inference for Physical AI Manipulation
* **Group**: 14
* **Member**: 陳柏宗, 張予綸, 莊詔允, 楊淯棋, 邱倫恩

## 2. Selected Tasks
* **Entry-level Tasks**: Cup stacking, Cutlery arrangement, and Toy block collection.
* **Advanced Task**: Cookie Stowage Task (Stowing specific cookie boxes into assigned drawers).

## 3. Ontology Design Explanation
Our ontology is designed to ground perceived physical objects into semantic concepts that a robot can query and reason about. We extend the baseline course ontology by not only modeling objects, roles, and grasping affordances, but also introducing advanced semantic rules. We explicitly distinguish between an object's type (e.g., `Cup`), its task role (e.g., `TargetObject`), its affordance (e.g., `GraspingAffordance`), and its physical instance. Furthermore, we designed two advanced inference rules (`StackableObject` and `OpenableObject`) alongside a custom object property (`assignedToContainer`) to model complex task success criteria semantically.

## 4. Modeled Objects and Affordances

| Object Instance | Object Type (Class) | Task Role | Affordance |
| :--- | :--- | :--- | :--- |
| `g14:blueCup01`, `g14:pinkCup01` | `cap:Cup` | `g14:role_target` | `cap:GraspingAffordance`, `cap:StackabilityAffordance` |
| `g14:knife01`, `g14:fork01` | `cap:Knife`, `cap:Fork` | `g14:role_target` | `cap:GraspingAffordance` |
| `g14:plate01` | `cap:Plate` | `g14:role_reference` | `cap:SupportAffordance` |
| `g14:block01`, `g14:block02`, `g14:block03` | `cap:ToyBlock` | `g14:role_collectable` | `cap:GraspingAffordance` |
| `g14:basket01` | `cap:Basket` | `g14:role_container` | `cap:ContainmentAffordance` |
| `g14:table01` (Advanced) | `g14:Table` | `g14:role_reference` | `cap:SupportAffordance` |
| `g14:drawer01`, `g14:drawer02`, `g14:drawer03` (Advanced) | `g14:Drawer` | `g14:role_container` | `cap:ContainmentAffordance`, `g14:OpenableAffordance` |
| `g14:cookieBox01`, `g14:cookieBox02`, `g14:cookieBox03` (Advanced) | `g14:CookieBox` | `g14:role_target` | `cap:GraspingAffordance` |

## 5. Namespace Policy
Following the course guidelines, we maintain strict separation of namespaces:
* **`cap:`** (`https://hcis.io/ontology/aicapstone/2026/`): Reserved for shared course vocabulary (e.g., `PhysicalObject`, `GraspingAffordance`).
* **`g14:`** (`https://hcis.io/ontology/aicapstone/2026/group14/`): Used for our group-specific extensions, advanced task classes (e.g., `g14:Drawer`), custom properties (`g14:assignedToContainer`), and all environment object instances (e.g., `g14:blueCup01`).

## 6. Instructions for Running the Queries

### Reasoning workflow in Protégé

1. Open Protégé.
2. Load `ontology/group-ontology.ttl`.
3. Confirm that the imported course ontology is available at `ontology/imports/course-affordance.ttl`.
4. Start an OWL 2 DL reasoner such as HermiT from the `Reasoner` menu.
5. Verify inferred class membership under the inferred class/individual views.
6. Export the inferred ontology as `ontology/inferred-results.ttl`.

### Query workflow in Protégé

1. Open `ontology/inferred-results.ttl` in Protégé.
2. Open the Snap SPARQL Query view:
   `Window` > `Views` > `Query views` > `Snap SPARQL Query`.
3. Run the query files under `queries/`:
   - `queries/graspable_objects.rq`
   - `queries/openable_objects.rq`
   - `queries/stackable_objects.rq`
4. The saved text outputs are available under `results/`.
5. Screenshots of the query results are available under `results/screenshots/`.

## 7. Expected Query Output

- **`queries/graspable_objects.rq`** retrieves objects inferred as `cap:GraspableObject`.
  The expected result contains 10 graspable objects: 2 cups, knife, fork, 3 toy blocks, and 3 cookie boxes.
  The plate, basket, table, and drawers are excluded because they are modeled as reference, support, containment, or openable objects rather than direct grasp targets.

- **`queries/openable_objects.rq`** retrieves objects inferred as `g14:OpenableObject`.
  The expected result contains `g14:drawer01`, `g14:drawer02`, and `g14:drawer03`.

- **`queries/stackable_objects.rq`** retrieves objects inferred as `g14:StackableObject`.
  The expected result contains `g14:blueCup01` and `g14:pinkCup01`.

Saved query outputs:
- `results/graspable_objects_output.txt`
- `results/openable_objects_output.txt`
- `results/stackable_objects_output.txt`

Screenshots:
- `results/screenshots/graspable_result_output.png`
- `results/screenshots/openable_result_output.png`
- `results/screenshots/stackable_result_output.png`

## 8. What is Inferred vs. Asserted
In our raw ontology (`group-ontology.ttl`), we only **asserted** the base types of objects (e.g., `g14:blueCup01 a cap:Cup`). We never manually asserted that any object is a `GraspableObject`, `StackableObject`, or `OpenableObject`. 
Through the `owl:equivalentClass` axioms and existential restrictions (`owl:someValuesFrom`), the OWL reasoner dynamically **inferred** these classifications based on the affordances assigned to each parent class. The results of these inferences are exported and saved in the `ontology/inferred-results.ttl` file.

## 9. Generation of `inferred-results.ttl`

1. Loaded `ontology/group-ontology.ttl` into Protégé.
2. Confirmed that the course ontology was available as an imported dependency from `ontology/imports/course-affordance.ttl`.
3. Activated the HermiT reasoner.
4. Verified inferred class membership for:
   - `cap:GraspableObject`
   - `g14:OpenableObject`
   - `g14:StackableObject`
5. Used `File` > `Export inferred axioms as ontology...`.
6. Selected class assertions and relevant logical axioms.
7. Saved the exported inferred graph as `ontology/inferred-results.ttl` using Turtle syntax.

## 10. Repository Files

Repository: https://github.com/Day-Aaron-661/semantic-affordance-grounding

Main files:
- Group ontology: `ontology/group-ontology.ttl`
- Inferred ontology: `ontology/inferred-results.ttl`
- Imported course ontology: `ontology/imports/course-affordance.ttl`
- Graspable query: `queries/graspable_objects.rq`
- Openable query: `queries/openable_objects.rq`
- Stackable query: `queries/stackable_objects.rq`
- Graspable output: `results/graspable_objects_output.txt`
- Openable output: `results/openable_objects_output.txt`
- Stackable output: `results/stackable_objects_output.txt`
- Screenshots: `results/screenshots/`
- Report: `report.pdf`
