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
| `blueCup01`, `pinkCup01` | `cap:Cup` | `role_target` | `GraspingAffordance`, `StackabilityAffordance` |
| `knife01`, `fork01` | `cap:Knife`, `cap:Fork` | `role_target` | `GraspingAffordance` |
| `plate01` | `cap:Plate` | `role_reference` | `SupportAffordance` |
| `block01`, `block02`, `block03` | `cap:ToyBlock` | `role_collectable`| `GraspingAffordance` |
| `basket01` | `cap:Basket` | `role_container` | `ContainmentAffordance` |
| `table01` (Adv) | `g14:Table` | `role_reference` | `SupportAffordance` |
| `drawer01~03` (Adv) | `g14:Drawer` | `role_container` | `ContainmentAffordance`, `OpenableAffordance` |
| `cookieBox01~03` (Adv)| `g14:CookieBox` | `role_target` | `GraspingAffordance` |

## 5. Namespace Policy
Following the course guidelines, we maintain strict separation of namespaces:
* **`cap:`** (`https://hcis.io/ontology/aicapstone/2026/`): Reserved for shared course vocabulary (e.g., `PhysicalObject`, `GraspingAffordance`).
* **`g14:`** (`https://hcis.io/ontology/aicapstone/2026/group14/`): Used for our group-specific extensions, advanced task classes (e.g., `g14:Drawer`), custom properties (`g14:assignedToContainer`), and all environment object instances (e.g., `g14:blueCup01`).

## 6. Instructions for Running the Query
1. Open Protégé and load `ontology/inferred-results.ttl`.
2. Ensure an OWL 2 DL Reasoner (e.g., HermiT) is started via the `Reasoner` menu.
3. Open the `Snap SPARQL Query` view (`Window` > `Views` > `Query views` > `Snap SPARQL Query`).
4. Copy the SPARQL code from `queries/graspable_objects.rq` (or `advanced_task.rq`).
5. Paste it into the query window and click **Execute**.

## 7. Expected Query Output
* **`graspable_objects.rq`**: Returns all 10 graspable objects (2 cups, knife, fork, 3 blocks, 3 cookie boxes) along with their object labels and task roles. Crucially, the plate, basket, table, and drawers are excluded.
* **`advanced_task.rq`**: Returns objects inferred as `g14:OpenableObject` (drawer01, drawer02, drawer03).

## 8. What is Inferred vs. Asserted
In our raw ontology (`group-ontology.ttl`), we only **asserted** the base types of objects (e.g., `g14:blueCup01 a cap:Cup`). We never manually asserted that any object is a `GraspableObject`, `StackableObject`, or `OpenableObject`. 
Through the `owl:equivalentClass` axioms and existential restrictions (`owl:someValuesFrom`), the OWL reasoner dynamically **inferred** these classifications based on the affordances assigned to each parent class. The results of these inferences are solidified in the `inferred-results.ttl` file.

## 9. Generation of `inferred-results.ttl`
1. Loaded `group-ontology.ttl` into Protégé and activated the HermiT reasoner.
2. Verified the inferred class hierarchy under the "Individuals by class (Inferred)" tab.
3. Navigated to `File` > `Export inferred axioms as ontology...`.
4. Selected **Class assertions (individual types)** along with other logical axioms.
5. Saved the output with the `Turtle` syntax format as `ontology/inferred-results.ttl`.

## 10. Repository Links
https://github.com/Day-Aaron-661/semantic-affordance-grounding

