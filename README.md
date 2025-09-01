![image](https://github.com/user-attachments/assets/6be68108-c954-4d15-b26e-11b3f61e71a3)






## Development Notes

This mod represents a comprehensive custom outfit and body overhaul for the character Lune. The project is currently in active development.

## Disclaimer

This is a fan-made modification. It is not affiliated with or endorsed by the original game developers or publishers.


##  FAQ: File Types Explained

When modding **Lune**, you will encounter several different Unreal Engine file formats. Understanding their purpose helps you know which ones matter when creating or replacing assets.

The most common are **`.pak` and `.pac`** files. These act as large container archives, similar to `.zip` files, and hold the game’s packaged assets such as textures, meshes, animations, and audio. `.pak` is the standard Unreal Engine archive format, while `.pac` is a variation used by certain publishers but serves the same role. Mods usually work by creating or overriding these archives so that new content replaces the original.

More recent Unreal Engine versions (UE 4.25+ and UE5) introduced **`.ucas` and `.utoc`** files, which work together as part of the virtualized asset system. The `.ucas` file contains the actual bulk asset data, while the `.utoc` file acts as the index or table of contents, telling the engine where specific chunks of data are located. The game always loads the `.utoc` first and then streams from `.ucas`. Both are required for assets to function properly.

You may also encounter **`.usmap`** files. These are not assets themselves, but metadata that define how Unreal Engine objects are structured. They contain property names, type mappings, and serialization layouts used for `.uasset` and `.uexp` files. Many games strip this data to make reverse-engineering harder, which is why external `.usmap` files are often needed. Tools such as FModel or UAssetGUI rely on `.usmap` to correctly parse and display assets; without it, the data appears scrambled or unreadable.

Finally, **`.pskx`** files represent skeletal meshes. This is an extended version of Unreal’s older `.psk` format and is typically produced when extracting models with tools like UE Viewer (UModel). `.pskx` files store 3D mesh geometry, bone hierarchies, skin weights, multiple UV sets, and normals. They are essential for character and outfit models, and are often paired with `.psa` files, which contain animation data. For mods that involve dress changes, `.pskx` files are the core format you’ll be working with when swapping or editing models.

In short, `.pak` and `.pac` are the high-level archives, `.ucas` and `.utoc` are Unreal’s newer bulk data and indexing system, `.usmap` provides the metadata needed to read and edit assets, and `.pskx` holds the actual 3D skeletal meshes used for models such as outfits.





### Skeleton Import Anomaly: Dual 'Root' References

During the asset pipeline process, specifically when handling the base `SK_UE5_Mannequin` skeleton, a structural inconsistency was identified. The original mannequin skeleton includes both:

* A bone named `root`
* A socket named `Root`

When importing the corresponding `.psk` file into **Blender**, sockets are implicitly interpreted and converted as bones. This behavior results in the creation of **two distinct bone entities**: one named `root` and another named `Root`. Despite being semantically distinct in case-sensitive environments, this duality leads to namespace conflicts upon re-import into Unreal Engine.

### Unreal Engine Bone Renaming Behavior

Unreal Engine, upon encountering the name conflict during import, resolves it by **automatically renaming** one of the conflicting bone references. In this case, the socket-derived bone is renamed to `Root1`. While this mitigates the direct naming issue, it introduces a non-trivial deviation in the skeleton hierarchy and potentially impacts animation retargeting, bone weight mapping, and physics asset behavior.
### Potential Impact on Runtime Behavior: Freezing During Jump Sequences

A runtime issue has been observed, specifically a **freeze or stutter during jump animations**. While conclusive evidence is pending, it is hypothesized that this may be related to the presence of redundant or misnamed bones (`root`, `Root`, `Root1`). The conflict may cause anomalies in animation blueprint execution, bone-driven controllers, or physics simulations associated with the character's vertical motion states.








