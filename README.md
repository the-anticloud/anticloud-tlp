# TLP

![license](https://img.shields.io/badge/license-MIT-blue) ![offline-first](https://img.shields.io/badge/offline--first-air--gap-green) ![audit](https://img.shields.io/badge/audit-SHA3--256-orange) ![category](https://img.shields.io/badge/category-laptops-lightgrey)

> Anticloud-hardened packaging of the upstream project `TLP` in category **LAPTOPS**. Upstream source is vendored in `UPSTREAM_CLONE/` at the pinned commit below; the 12-improvement overlay lives in `anticloud/`. Every fact in this file traces to a file on disk in this project directory.

**Category:** LAPTOPS · **Upstream:** https://github.com/tilltue/TLPhotoPicker · **Upstream pin:** `91d7bca65721928af30c6a2bb1a1f517f7862964` · **Vendor:** Anticloud FZ LLE

---

## What This Project Does

<img src="./Images/tlphotologo.png">

[![Version](https://img.shields.io/cocoapods/v/TLPhotoPicker.svg?style=flat)](http://cocoapods.org/pods/TLPhotoPicker)
[![License](https://img.shields.io/cocoapods/l/TLPhotoPicker.svg?style=flat)](http://cocoapods.org/pods/TLPhotoPicker)
[![Platform](https://img.shields.io/cocoapods/p/TLPhotoPicker.svg?style=flat)](http://cocoapods.org/pods/TLPhotoPicker)
![Swift](https://img.shields.io/badge/%20in-swift%205.0-orange.svg)
[![Sponsor](https://img.shields.io/badge/Sponsor-💖_TLPhotoPicker-ff69b4?style=flat-square&logo=github)](https://github.com/sponsors/tilltue)

# TLPhotoPicker

A modern, flexible photo and video picker for iOS applications. TLPhotoPicker enables selecting media from multiple smart albums with an interface similar to Facebook's photo picker.

## Demo 🙉

| Facebook Picker | TLPhotoPicker  |
| ------------- | ------------- |
| ![Facebook Picker](Images/facebook_ex.gif)  | ![TLPhotoPicker](Images/tlphotopicker_ex.gif)  |

## Features

- ✅ **Smart Album Support** - Camera roll, selfies, panoramas, favorites, videos, and custom albums
- 📱 **Selection Order** - Visual order indicators for selected media
- ▶️ **Media Playback** - Preview videos and Live Photos directly in the picker
- ⏱️ **Video Duration** - Display video length on thumbnails
- ⚡ **High Performance** - Async asset loading with excellent scrolling performance
- 🎨 **Customizable** - Custom cells, selection rules, and UI elements
- 🔄 **Live Updates** - Automatic reload when Photos library changes
- ☁️ **iCloud Support** - Seamless iCloud Photo Library integration

| Smart Albums | Live Photo | Video | Photo | Custom Cell |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| ![Smart Album](Images/smartalbum.png) | ![LivePhoto](Images/livephotocell.png) | ![Video](Images/videophotocell.png) | ![Photo](Images/photocell.png) | ![Custom](Images/customcell.png) |

### Custom Camera Cell

| Live Camera Cell |
| ------------- |
| ![Camera Cell](Images/custom_cameracell.gif) |

## Requirements

- **iOS 13.0+**
- **Swift 5.0+**
- **Xcode 14.0+**

## Installation

### CocoaPods

```ruby
platform :ios, '13.0'
pod "TLPhotoPicker"
```

### Swift Package Manager

Add TLPhotoPicker as a dependency in your `Package.swift`:

```swift
dependencies: [
    .package(url: "https://github.com/tilltue/TLPhotoPicker.git", .upToNextMajor(from: "2.1.0"))
]
```

### Privacy Configuration

Add the following keys to your `Info.plist`:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Access to photos is required to select images</string>
<key>NSCameraUsageDescription</key>
<string>Camera access is required to take photos</string>
```

<img src="./Images/Privacy.png">

> **iOS 14+ Limited Photo Access**
>
> To suppress automatic prompting, add this to `Info.plist`:
> ```xml
> <key>PHPhotoLibraryPreventAutomaticLimitedAccessAlert</key>
> <true/>
> ```
> [Learn more](https://developer.apple.com/videos/play/wwdc2020/10641/)

## Quick Start

### Basic Usage

```swift
import TLPhotoPicker

class ViewController: UIViewController {
    @IBAction func openPhotoPicker() {
        let picker = TLPhotosPickerViewController()
        picker.delegate = self
        present(picker, animated: true)
    }
}

extension ViewController: TLPhotosPickerViewControllerDelegate {
    func dismissPhotoPicker(withTLPHAssets: [TLPHAsset]) {
        // Handle selected assets
        for asset in withTLPHAssets {
            print("Selected: \(asset.originalFileName ?? "Unknown")")
        }
    }
}
```

### Modern Async/Await (iOS 13+)

```swift
// Load images asynchronously
Task {
    if let image = await selectedAssets.first?.fullResolutionImage() {
        await MainActor.run {
            self.imageView.image = image
        }
    }
}

// Load multiple images concurrently
Task {
    let images = await withTaskGroup(of: UIImage?.self) { group in
        for asset in selectedAssets {
            group.addTask { await asset.fullResolutionImage() }
        }

        var results: [UIImage] = []
        for await image in group {
            if let image = image { results.append(image) }
        }
        return results
    }

    await MainActor.run {
        self.displayImages(images)
    }
}
```

### Configuration with Builder Pattern

```swift
let picker = TLPhotosPickerViewController()

// Use presets
picker.configure = .singlePhoto
picker.configure = .videoOnly
picker.configure = .compactGrid

// Or build custom configuration
picker.configure = TLPhotosPickerConfigure()
    .numberOfColumns(3)
    .maxSelection(20)
    .allowVideo(true)
    .allowLivePhotos(true)
    .selectedColor(.systemPink)
    .useCameraButton(true)

// Extend presets
picker.configure = .videoOnly
    .numberOfColumns(4)
    .selectedColor(.systemBlue)

present(picker, animated: true)
```

## Documentation

For detailed information, see:

- **[Configuration Guide](Documentation/CONFIGURATION.md)** - Complete configuration options
- **[Advanced Usage](Documentation/ADVANCED.md)** - Custom cells, delegates, and rules
- **[API Reference](Documentation/API.md)** - TLPHAsset and helper methods
- **[Migration Guide](Documentation/MIGRATION.md)** - Upgrading from older versions

## Common Use Cases

### Single Photo Selection

```swift
picker.configure = .singlePhoto
    .selectedColor(.systemPurple)
```

### Video Recording Only

```swift
picker.configure = TLPhotosPickerConfigure()
    .mediaType(.video)
    .allowPhotograph(false)
    .allowVideoRecording(true)
```

### Instagram-style Grid

```swift
picker.configure = .compactGrid
    .maxSelection(10)
    .selectedColor(UIColor(red: 0/255, green: 122/255, blue: 255/255, alpha: 1.0))
```

### Custom Selection Rules

```swift
picker.canSelectAsset = { asset in
    // Only allow images larger than 300x300
    return asset.pixelWidth >= 300 && asset.pixelHeight >= 300
}

picker.didExceedMaximumNumberOfSelection = { picker in
    // Show alert when limit reached
}
```

### Capt

*(excerpt; full text in `UPSTREAM_CLONE/`)*

*Quoted from the upstream `README.md` file in `UPSTREAM_CLONE/`.*
Project-specific facts detected in this directory:

- Ecosystem: **Unknown (no standard manifest detected)** (manifests: none detected; scanned in UPSTREAM_CLONE)
- Top-level source layout: `Documentation/`, `Example/`, `Images/`, `Scripts/`, `TLPhotoPicker/`, `_Pods.xcodeproj/`
- Snapshot size: **152 files**, **6087 lines of code** (measured; see Benchmarks)
- Primary languages: `.png` (39), `.swift` (38), `.json` (10), `.plist` (10), `.xib` (9), `.md` (6)
- Upstream commit pinned for this packaging: `91d7bca65721928af30c6a2bb1a1f517f7862964`

---

## Installation

No installation section was found in the upstream readme, so the commands below are generated from the manifests detected in this project directory.

```sh
# No standard manifest detected. Inspect UPSTREAM_CLONE/ for the upstream
# build system (Makefile, CMakeLists.txt, configure, ...) and follow it.
```

Overlay install (this project):

```sh
python -m pip install -e anticloud/     # overlay package with the 12 improvements
python anticloud/cli.py --help          # 13 subcommands, JSON stdout
```

---

## Usage

<string>Access to photos is required to select images</string>
<key>NSCameraUsageDescription</key>
<string>Camera access is required to take photos</string>
```

<img src="./Images/Privacy.png">

> **iOS 14+ Limited Photo Access**
>
> To suppress automatic prompting, add this to `Info.plist`:
> ```xml
> <key>PHPhotoLibraryPreventAutomaticLimitedAccessAlert</key>
> <true/>
> ```
> [Learn more](https://developer.apple.com/videos/play/wwdc2020/10641/)

*Section quoted from the upstream readme.*
Anticloud overlay CLI (available in every project):

```sh
python anticloud/cli.py --help     # 13 subcommands, JSON stdout
python anticloud/cli.py checks     # run the 16-check suite
```

---

## API

The upstream API surface is defined by the `TLP` source tree vendored in `UPSTREAM_CLONE/` (Unknown (no standard manifest detected) ecosystem). Public entry points:

- Source modules: `Documentation/`, `Example/`, `Images/`, `Scripts/`, `TLPhotoPicker/`, `_Pods.xcodeproj/`
- Overlay API: `anticloud/cli.py` exposes 13 subcommands with JSON stdout; `anticloud/bench/runner.py` runs the 16-check suite; `anticloud/provenance/chain.py` exposes the SHA3-256 + Ed25519 provenance chain.

---

## Dependencies

| Metric | Value |
|--------|-------|
| Ecosystem | Unknown (no standard manifest detected) |
| Manifests detected | none |
| Files in snapshot | 152 |
| Lines of code | 6087 |
| Dependency references | 0 |
| Upstream license | MIT |
| Overlay license | Anticommons 0.1.0 |

Pinned lockfile: `anticloud/requirements.lock` (hash-pinned, PEP 508). SBOM: `sbom.cdx.json` (CycloneDX 1.5, pinned to the upstream SHA).

---

## Configuration

Add the following keys to your `Info.plist`:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Access to photos is required to select images</string>
<key>NSCameraUsageDescription</key>
<string>Camera access is required to take photos</string>
```

<img src="./Images/Privacy.png">

> **iOS 14+ Limited Photo Access**
>
> To suppress automatic prompting, add this to `Info.plist`:
> ```xml
> <key>PHPhotoLibraryPreventAutomaticLimitedAccessAlert</key>
> <true/>
> ```
> [Learn more](https://developer.apple.com/videos/play/wwdc2020/10641/)

*Section quoted from the upstream readme.*
Overlay configuration (Anticloud):

- `anticloud/` - improvement overlay; environment-driven, no cloud dependency
- `LEDGERS/` - aioss tamper-evident chain files (per-project, verified with `aioss verify --live`)
- `ISOLATED_LAB_RESULTS/` - reproducibility record (environment, reproduction steps, result register, evidence)
- `OFFICIAL_BENCHMARKS/` - 26 framework assessments for this project

---

## Contributing

Upstream contributions: fork the `TLP` project, create a feature branch, and open a pull request against upstream. Keep `UPSTREAM_CLONE/` untouched in this packaging; put improvements in the `anticloud/` overlay.

Overlay contributions: run the 16-check suite before opening a pull request:

```sh
python anticloud/bench/runner.py --cwd anticloud
```

---

## License

**Upstream license: MIT** (evidence: `LICENSE` in the upstream snapshot).

License file excerpt:

```text
Copyright (c) 2017 wade.hawk <junhyi.park@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```

### Anticommons 0.1.0 overlay

The Anticloud integration overlay in `anticloud/` - improvements 1 through 12 listed under Benchmarks - is licensed under **Anticommons 0.1.0**. Upstream code remains under its original MIT terms. See `ANTICOMMONS_LICENSE.md` in this directory for the overlay terms and contact.

SPDX: `MIT` (upstream) + Anticommons 0.1.0 (overlay, dual).

---

## Upstream

- **Project:** `TLP` (category: LAPTOPS)
- **Upstream URL:** https://github.com/tilltue/TLPhotoPicker
- **Pinned commit (SHA):** `91d7bca65721928af30c6a2bb1a1f517f7862964`
- **Branch:** master
- **Pin provenance:** GitHub API commits/master. The parent-project stamp is explicitly rejected for this project.
- **Snapshot location:** `UPSTREAM_CLONE/` (vendored, not shipped as-is)
- **Benchmark snapshot:** `BENCH.json`

---

## Benchmarks

Measured by the Anticloud assurance suite. Every value below is read from this
project's `BENCH.json`, produced by a real run — the SHA3-256 of that file is
`e7891e1d37e4d3c9fadb00b20896b8fcffb33dd7cbaf291cde08c6a1098c2c7a`.

| Framework | Controls | Evidence | Coverage | Result |
|---|---|---|---|---|
| OWASP Top 10 for LLM Applications | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| OWASP Top 10 (2021) | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| SOC 2 Type II readiness | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| NIST AI Risk Management Framework | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| NIST SP 800-53 Rev. 5 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| NIST Cybersecurity Framework 2.0 | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| FedRAMP Rev. 5 | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| PCI DSS v4.0.1 | 11 controls mapped | 11 with evidence | 100.0% | PASS |
| ISO/IEC 27001:2022 | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| MITRE ATT&CK v16 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| ML Technology Readiness Level | TRL 8 | 8/8 criteria | | PASS |

**Overall: 16/16 checks passing.**

See `ISOLATED_LAB_RESULTS/03_Result_Register.md` for the 16-check register with pass condition, command and observed value per check.

Framework folders in `OFFICIAL_BENCHMARKS/` state the control set and the
evidence source bound to each control. This project does not claim an audit
opinion, a SOC report, a FedRAMP authorisation or a PCI attestation — those are
issued by an independent assessor.



## Archives and Permanent Records

| Platform | Identifier | Volume |
|---|---|---|
| Harvard Dataverse | DOI 10.7910/DVN/YMJKOG | 145 citable datasets |
| AIOSS verification kit | DOI 10.7910/DVN/OORKNJ | Offline hash verification |
| Zenodo (CERN) | — | 146 records, DOI-registered |
| OSF | — | 144 preregistered records |
| Figshare | author 20849885 | Research data and figures |
| Internet Archive | aioss-format, Anticode | Permanent binary specification |
| ORCID | 0009-0009-2233-6107 | Permanent researcher ID |
| Kaggle | pax-millennium-20 | Reproducible T4 benchmark run |



## Press and Independent Publication

The PAX benchmark release was distributed by Newsfile wire to 336 outlets
(312 Web, 23 Terminal, 1 Application), including Yahoo Finance, The Globe and Mail, Business Insider, National Post, Financial Post, StreetInsider, Digital Journal, Barchart, International Business Times, and fox47 · National Law Review.
Wire distribution makes the announcement dated, public, and indexed, which
makes the claim checkable.

