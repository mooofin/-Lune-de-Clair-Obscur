![image](https://github.com/user-attachments/assets/6be68108-c954-4d15-b26e-11b3f61e71a3)






## Development Notes

This mod represents a comprehensive custom outfit and body overhaul for the character Lune. The project is currently in active development.

## Disclaimer

This is a fan-made modification. It is not affiliated with or endorsed by the original game developers or publishers.
### Skeleton Import Anomaly: Dual 'Root' References

During the asset pipeline process, specifically when handling the base `SK_UE5_Mannequin` skeleton, a structural inconsistency was identified. The original mannequin skeleton includes both:

* A bone named `root`
* A socket named `Root`

When importing the corresponding `.psk` file into **Blender**, sockets are implicitly interpreted and converted as bones. This behavior results in the creation of **two distinct bone entities**: one named `root` and another named `Root`. Despite being semantically distinct in case-sensitive environments, this duality leads to namespace conflicts upon re-import into Unreal Engine.

### Unreal Engine Bone Renaming Behavior

Unreal Engine, upon encountering the name conflict during import, resolves it by **automatically renaming** one of the conflicting bone references. In this case, the socket-derived bone is renamed to `Root1`. While this mitigates the direct naming issue, it introduces a non-trivial deviation in the skeleton hierarchy and potentially impacts animation retargeting, bone weight mapping, and physics asset behavior.
### Potential Impact on Runtime Behavior: Freezing During Jump Sequences

A runtime issue has been observed, specifically a **freeze or stutter during jump animations**. While conclusive evidence is pending, it is hypothesized that this may be related to the presence of redundant or misnamed bones (`root`, `Root`, `Root1`). The conflict may cause anomalies in animation blueprint execution, bone-driven controllers, or physics simulations associated with the character's vertical motion states.








