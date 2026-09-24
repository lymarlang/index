# Lymar Central Package Index

The official Git-backed package registry index for the [Lymar programming language](https://github.com/lymarlang) and the **Lyra** package manager.

This repository follows the **Git-Backed Central Index model** (popularized by Homebrew, Cargo, and CocoaPods). It stores package metadata manifests as human-readable, auditable NOL (Nested Object Language) files, ensuring 100% decentralized storage, cryptographically verified integrity, and community-driven package publication via GitHub Pull Requests.

---

## 📁 Repository Structure

```text
index/
├── config.nol               # Registry configuration and versioning
├── .github/
│   └── workflows/
│       └── validate.yml     # Automated CI verification for package submissions
└── packages/                # Package manifest database
    └── <package-name>.nol   # Package metadata and version history
```

For large-scale registries, packages can optionally be sharded as `packages/<shard>/<package-name>.nol` (e.g. `packages/n/nol.nol`).

---

## 📦 Package Manifest Specification

Every package in `packages/<name>.nol` contains the package metadata and a dictionary of all published versions, download locations, and SHA256 checksums.

Example `packages/nol.nol`:
```nol
package: {
  name: "nol",
  description: "Nested Object Language (NOL) parser and serializer for Lymar",
  repository: "https://github.com/lymarlang/nol.git"
}

versions: {
  "0.1.0": {
    git: "https://github.com/lymarlang/nol.git",
    tag: "v0.1.0",
    hash: "sha256:e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
    dependencies: {}
  }
}
```

### Fields:
- `package.name`: Unique package name (must match filename `<name>.nol`).
- `package.description`: Concise summary of the library.
- `package.repository`: Canonical source Git repository URL.
- `versions.<semver>`: Specific SemVer release version (e.g., `0.1.0`, `1.2.3`).
  - `git`: Git repository URL containing the release.
  - `tag`: Git tag corresponding to this release (defaults to `v<version>`).
  - `url`: Optional direct tarball download URL (e.g., GitHub Releases asset).
  - `hash`: `sha256:<digest>` integrity checksum of the release archive.
  - `dependencies`: Map of dependency names to SemVer constraints (e.g., `"^1.0.0"`).

---

## 🚀 How to Publish a Package

### Method 1: Using Lyra CLI (Recommended)

1. Inside your Lymar project with a valid `lymar.nol`:
   ```bash
   lyra publish
   ```
2. Lyra packages your project, computes the SHA256 checksum, and generates or updates the package manifest under your local index (`~/.lyra/index/packages/<name>.nol`).
3. Follow the CLI prompt:
   ```bash
   cd ~/.lyra/index
   git checkout -b publish-<name>-<version>
   git add packages/<name>.nol
   git commit -m "Publish <name> <version>"
   git push origin publish-<name>-<version>
   ```
4. Open a Pull Request to `https://github.com/lymarlang/index.git`.

### Method 2: Manual Pull Request

1. Fork `https://github.com/lymarlang/index.git`.
2. Add or update `packages/<your-package>.nol`.
3. Commit and submit a Pull Request.
4. Automated GitHub CI will validate the manifest and, upon merge, your package becomes immediately installable by all Lymar developers!

---

## 📥 Installing Packages

Add the dependency to your project's `lymar.nol`:

```nol
dependencies: {
  nol: "^0.1.0"
}
```

Or run:
```bash
lyra add nol
```

Then update or run your project:
```bash
lyra update
lyra run
```

---

## 🛡️ License

The Lymar package index is available under the [MIT License](LICENSE).
