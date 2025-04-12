# Revit to Unity Material & Context Exporter

**Developed by:** Faraz Khojasteh Far
**License:** MIT License (See LICENSE file)
**Current Status:** v1.0.0 - Release

This project provides a workflow and tools to export models from Autodesk Revit (v2025) to Unity, attempting to preserve materials and textures more accurately than standard FBX by exporting material data and element context separately. Materials are assigned in Unity based on matching GameObject names to exported context.

## Features

*   Exports Revit material data (Color, Textures, basic PBR values) to `materials.json`.
*   Exports associated Element context (Name, Category, Type, Family, Material Name).
*   Copies referenced texture files to a `Textures` subfolder.
*   Includes Unity Editor script to automatically create materials and attempt assignment based on context.
*   Glass material override for basic transparency in Unity.

## Requirements

*   **Revit 2025** (Built for .NET 8). *May not work with other versions.*
*   **Unity 2022.3 LTS** or later (Script uses Standard Render Pipeline shader by default).
*   Windows Operating System.

## Installation

This process involves installing files in two places: your Documents folder (temporarily) and the actual Revit Addins folder.

**1. Run the Installer:**

*   Download the `RevitMaterialExporterSetup-v1.0.0.msi` file.
*   Double-click the `.msi` file to run the installer.
*   Follow the simple setup prompts (Click Next -> Install -> Finish).
*   This will create a folder named `RevitMaterialExporterInstallFiles` inside your main **Documents** folder (e.g., `C:\Users\<YourUsername>\Documents\RevitMaterialExporterInstallFiles`).
*   Inside this folder, you will find the necessary files (`RevitMaterialExporter.dll`, `RevitMaterialExporter.addin`, and an instruction text file).

**2. Manually Move Add-in Files:**

*   **Close Revit 2025 completely.**
*   Open Windows File Explorer.
*   Navigate to the folder created by the installer: `Documents\RevitMaterialExporterInstallFiles`.
*   Open **another** File Explorer window.
*   In the address bar of the second window, paste the following path and press Enter:
    `%APPDATA%\Autodesk\Revit\Addins\2025`
    _(This usually goes to C:\Users\<YourUsername>\AppData\Roaming\Autodesk\Revit\Addins\2025)_
*   **Select BOTH** files (`RevitMaterialExporter.dll` and `RevitMaterialExporter.addin`) inside the `RevitMaterialExporterInstallFiles` folder.
*   **MOVE** (Cut and Paste, or Drag) these two selected files into the `%APPDATA%\...\Addins\2025` folder you opened in the second window.
*   Start Revit 2025. The export button should now appear under the Add-Ins tab.

**3. Install Unity Package:**

*   Download the `RevitContextProcessor_v1.0.0.unitypackage` file (or similar name).
*   Open your Unity Project (must use Standard RP, or edit script for URP/HDRP).
*   Go to the Unity menu: `Assets -> Import Package -> Custom Package...`.
*   Browse to and select the downloaded `.unitypackage` file.
*   Click `Import`. Ensure it places `RevitMaterialProcessor.cs` inside an `Assets/Editor` folder.

## Workflow / How to Use

1.  **Revit:**
    *   Open your Revit project and a relevant 3D View.
    *   Go to `Add-Ins` tab -> `External Tools`.
    *   Click the "Revit Material Exporter" button.
    *   The plugin will automatically create `materials.json` and a `Textures` subfolder inside your main **Documents** folder.
2.  **Revit (FBX):**
    *   Go to `File -> Export -> FBX`.
    *   Save the FBX file **inside the same BASE FOLDER** in the previous step.
3.  **Unity:**
    *   Drag the entire **BASE FOLDER** (containing `materials.json`, `Textures`, and your `.fbx`) into your Unity Project's `Assets` window.
    *   Wait for import.
    *   **Select the imported FBX asset**, go to Inspector -> Model tab, check **`Read/Write Enabled`**, click Apply.
    *   If using normal maps, configure their `Texture Type` in the Inspector.
4.  **Unity (Process):**
    *   Go to `Revit Tools -> Process Materials via Enhanced Context`.
    *   Drag the imported FBX Prefab onto the `Revit Model Root` field.
    *   Drag the imported `materials.json` TextAsset onto the `Materials JSON File` field.
    *   Adjust matching options if needed (Approximate=ON is usually best).
    *   Click `Process Materials via Enhanced Context`.
    *   Check results and Console log.

## Known Issues & Limitations

*   Context matching isn't perfect; some objects might remain unassigned if names differ too much between Revit context and imported GameObject names. Check Console warnings.
*   Requires manual file moving after running the Revit add-in installer.
*   Only tested with Revit 2025 and Unity Standard Render Pipeline. Shader modifications needed for URP/HDRP.
*   Material translation is approximate.

## Uninstallation

1.  **Revit Add-in:** Delete `RevitMaterialExporter.dll` and `RevitMaterialExporter.addin` from `%APPDATA%\Autodesk\Revit\Addins\2025\`. You can also uninstall "Revit Material Exporter v1.0 (Files Installer)" from Windows Apps & features (this removes the entry but likely leaves the files in Documents). Optionally delete the folder created in Documents.
2.  **Unity Package:** Delete `RevitMaterialProcessor.cs` from `Assets/Editor` in your Unity project. Delete generated materials folder if desired.

## License

MIT License (See LICENSE file)
