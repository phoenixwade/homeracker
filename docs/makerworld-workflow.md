# 🌐 MakerWorld Description Workflow

## 📌 What

How to manage MakerWorld model descriptions as git-tracked `DESCRIPTION.md` files and publish them to MakerWorld.

## 🤔 Why

- ✅ Keep MakerWorld descriptions versioned, reviewable, and easy to update alongside model changes
- 🖼️ Store images in the shared assets repo so source repos stay lean and links remain stable
- 🔁 Use one Markdown source that can be edited in Git and republished to MakerWorld as HTML

## 🔧 How

### Extract (MakerWorld → Git)

Extract a description from a MakerWorld model page:

1. Open the MakerWorld model URL and copy the description into `DESCRIPTION.md`.
2. Download the description images to the **assets repo** and replace their source URLs with the corresponding raw GitHub URLs.
3. Commit `DESCRIPTION.md` to the source repo and the images to `kellerlabs/assets`.

### Publish (Git → MakerWorld)

```bash
pip install -r requirements.txt
python cmd/export/md-to-mw.py models/<name>/makerworld/DESCRIPTION.md
```

This generates `DESCRIPTION.html` (gitignored). Open it in a browser, `Ctrl+A`, `Ctrl+C`, paste into MakerWorld's description editor.

### Edit

1. Edit `DESCRIPTION.md` directly in the source repo
2. Re-run `md-to-mw.py` to regenerate HTML
3. Re-paste into MakerWorld

### Update (After Model Changes)

Refresh a description after a release:

1. Read `CHANGELOG.md` and identify changes relevant to the model.
2. Update the description's changelog entries and feature bullets.
3. Check the assets repo for new images and add them where appropriate.
4. Add or update the `updated: YYYY-MM-DD` frontmatter field.
5. Publish with `md-to-mw.py` (see above).

## 📁 Image & Layout Formatting

> ⚠️ **Cross-repo workflow**: This process uses both the source repo and [`kellerlabs/assets`](https://github.com/kellerlabs/assets). Maintainers push images directly to `assets/main`. Outside collaborators must open a PR on the assets repo for image changes.

Images are stored in **[kellerlabs/assets](https://github.com/kellerlabs/assets)**, not in the source repos.

```
https://raw.githubusercontent.com/kellerlabs/assets/main/<repo>/models/<name>/makerworld/images/<file>
```

- Use `<img>` tags with `width` (no `height`) so images scale proportionally
- Use `<h2 style="text-align: center">` and `<p style="text-align: center">` for centered elements
- These HTML blocks render correctly on GitHub and pass through to `md-to-mw.py`

`md-to-mw.py` passes absolute URLs through unchanged. Base64 embedding only applies to local relative paths.

### Create (New Description)

Create a new `DESCRIPTION.md` from scratch:

1. Gather the model details and verify images in the assets repository.
2. Create `DESCRIPTION.md` and enhance `CUSTOMIZATION.md` with the verified images.
3. Optionally publish with `md-to-mw.py` (see above).

## 📚 References

- [image-hosting-assets-repo](decisions/image-hosting-assets-repo.md): why images live in a separate repo
- `cmd/export/md-to-mw.py`: conversion script
