---
description: Overview page on meshes
---

# Meshes

## Definition&#x20;

**In the context of Cyberpunk**, a mesh is an entity that holds the following information:

* the 3d object (as a number of **submeshes**)
* movement and deform information ([**rigging**](../3d-modelling/meshes-and-armatures-rigging.md) for the **rig**/**armature**, and **weights**)
* [Materials](meshes.md#materials) (as in which part of the 3d object will look how)

## Materials

{% hint style="info" %}
This page only contains mesh-specific information. Find more details on materials [here](meshes.md#materials).&#x20;
{% endhint %}

This is how to determine which parts of the mesh have which material. For a :

<figure><img src="../../.gitbook/assets/materials_explanation.png" alt=""><figcaption><p>Example: A mesh with two materials, one of them a local instance, one of them an external .mi file</p></figcaption></figure>

### ChunkMaterials

You assign materials based on the "chunks" (the individual submeshes) inside a mesh. Open the mesh file in Wolvenkit and open the "appearances" array, then make sure that each of your submeshes has an entry inside the array.

<figure><img src="../../.gitbook/assets/mesh_material_appearance.png" alt=""><figcaption><p>You may have to create additional entries in "chunkMaterials": Either duplicate an existing entry from the right-click menu, or select the array and use the yellow (+) in the side panel.</p></figcaption></figure>

### Material definition

Materials are defined in the array **`materialEntries`** inside your mesh:

<figure><img src="../../.gitbook/assets/materials_materialentries_overview.png" alt=""><figcaption><p>For a detailed example, see <a href="materials-how-to-configure-them/re-using-materials-.mi.md#maximally-lazy-external-materials">re-using materials</a></p></figcaption></figure>

| Property        | Description                                                                                                                                                                                                                                                                                                                                                                                                                            |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| index           | **numerical index** of corresponding material in target list (as defined by `isLocalInstance`)                                                                                                                                                                                                                                                                                                                                         |
| isLocalInstance | <p>Selects the material target list.<br><strong>True:</strong> <a href="meshes.md#materialinstance-the-local-material">local material</a> in <code>localMaterialBuffer.materials</code> or <code>preloadLocalMaterialInstances</code><br><strong>False:</strong> <a href="meshes.md#material-reference-a-material-somewhere-else">material reference</a> in<code>externalMaterials</code> or <code>preloadExternalMaterials</code></p> |
| name            | **unique** name of material, used to select the material via `chunkMaterial`                                                                                                                                                                                                                                                                                                                                                           |

#### Preload… what?

Many of CDPR's early meshes use `preloadLocalMaterialInstances` instead of `localMaterialBuffer.materials`. As far as we are concerned, you can use the two interchangeably, **but**:&#x20;

If you are using **a mix of local and external materials**, you **must** use the corresponding list:

| local                           | external                   |
| ------------------------------- | -------------------------- |
| `localMaterialBuffer.materials` | `externalMaterials`        |
| `preloadLocalMaterialInstances` | `preloadExternalMaterials` |

If you do not, then the materials outside of `preload`… will appear as transparent the first 1-2 times you trigger your item's appearance.

### MaterialInstance: The local material

The materials themselves are inside the array `localMaterialBuffer.materials` (or `preloadLocalMaterials` in case of older meshes).&#x20;

{% hint style="success" %}
You can't go wrong by using those. However, if you don't have any properties that are unique to your mesh or appearance (for example a custom normal map), you might consider [creating and using an external material instead](materials-how-to-configure-them/re-using-materials-.mi.md).
{% endhint %}

A material instance looks like this:

<figure><img src="../../.gitbook/assets/material_docu_material_instance.png" alt=""><figcaption><p>baseMaterial picks the material (shader), while "values" contains <a href="meshes.md#checking-material-properties">properties</a> to adjust it.</p></figcaption></figure>

{% hint style="info" %}
For an overview of materials that you might want to use for something, check [here](../references-lists-and-overviews/cheat-sheet-materials.md).&#x20;

For how to find out which properties a material has, check [here](materials-how-to-configure-them/#checking-material-properties).
{% endhint %}

### Material reference: [reusing materials](materials-how-to-configure-them/re-using-materials-.mi.md#maximally-lazy-external-materials)

A relative path to an external material, usually encapsulated in a [.mi file](materials-how-to-configure-them/re-using-materials-.mi.md#.mi-files-to-the-rescue). Use this if you don't need to add extra properties.