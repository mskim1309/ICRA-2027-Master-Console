# ICRA 2027 Master Console

This repository contains the design data, robot descriptions, manufacturing bill of materials, and assembly documentation for the master console presented in the accompanying ICRA 2027 paper. It is intended to support inspection, fabrication, and reproducible use of the hardware described in the paper.

> **Release status:** This repository is being prepared for the paper submission/review release. The final archival release will include the files and documentation listed below. Please use a tagged release when reproducing reported results.

## Repository layout

```text
.
├── CAD/                         # Native CAD source files
│   ├── distal_wrist/             # Distal-wrist mechanism
│   ├── grasp_module/             # Grasp/input module
│   ├── flange/                   # Robot/interface flange
│   └── console_frame/            # Main console frame and structural parts
├── URDF/                         # Robot descriptions for visualization/simulation
│   └── piper_with_custom_gripper.urdf         # PiPER arm + custom gripper urdf
├── STL/                          # Exported printable/manufacturable meshes
├── BOM/
│   └── BOM.xlsx                  # Purchased parts, quantities, and specifications
├── ASSEMBLY/
│   └── assembly_guide.pdf        # Assembly procedure and drawings
└── README.md
```

## Contents and intended use

| Directory | Contents | Intended use |
| --- | --- | --- |
| `CAD/` | Editable source models, organized by mechanical subassembly | Design inspection and modification |
| `STL/` | Mesh exports corresponding to released parts | Fabrication/3D-print preparation |
| `BOM/` | Part numbers, quantities, specifications, and procurement notes | Hardware sourcing |
| `ASSEMBLY/` | Ordered assembly instructions, fasteners, and reference drawings | Mechanical assembly |
| `URDF/` | Left and right kinematic descriptions | Visualization, simulation, and software integration |

## Reproducing the hardware

1. Review `BOM/BOM.xlsx` and obtain the listed purchased components before fabrication.
2. Fabricate the released parts from `STL/`, following the material, process, tolerance, and post-processing notes in the assembly guide.
3. Assemble the console using `ASSEMBLY/assembly_guide.pdf`. Do not infer dimensions or fasteners from mesh files when the guide specifies them.
4. Load the appropriate file from `URDF/` to verify the left/right configuration, link frames, joint axes, and mesh paths before connecting it to control software.
5. Record the repository tag/commit and any substitutions or fabrication deviations when reporting results.

## Important notes

- CAD source files are the authoritative editable design data. STL files are exports for fabrication and may be unsuitable for dimensional edits.
- All dimensions, units, coordinate-frame conventions, joint limits, mesh paths, and software dependencies will be documented with the final release. Validate them in your target simulator before operating hardware.
- The left and right consoles are separate URDF models. Do not assume that mirroring one file produces the correct frame conventions or joint signs for the other.
- This repository provides research hardware data only. Fabrication, integration, operation, and any safety assessment remain the responsibility of the user.

## Versioning and reproducibility

The version of this repository associated with the paper will be identified by a release tag. For a reproducible build or evaluation, cite the paper and record:

- repository URL and release tag (or commit SHA);
- CAD/URDF revision;
- BOM revision and any substituted components; and
- fabrication method, material, print settings, and post-processing changes.

## Citation

If you use this hardware, design data, or URDF models in academic work, please cite the associated paper:

```bibtex
@inproceedings{<citation-key>,
  title     = {<paper title>},
  author    = {<authors>},
  booktitle = {Proceedings of the IEEE International Conference on Robotics and Automation (ICRA)},
  year      = {2027},
  note      = {Paper submission / preprint; update with the final bibliographic record upon publication}
}
```

The final release will replace the placeholders above with the published citation and persistent repository URL.

## License and contact

License terms and a contact address will be added before public release. Until then, the contents are provided only for the paper-submission/review purpose; redistribution, fabrication, and commercial use require permission from the authors.
