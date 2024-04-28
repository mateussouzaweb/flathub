# Flatpak Build for NiceDeck

Use the commands below to run development builds:

```bash
# Install dependencies
flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo 
flatpak install -y flathub org.flatpak.Builder

# Validate files
flatpak run --command=flatpak-builder-lint org.flatpak.Builder manifest com.mateussouzaweb.NiceDeck.yml
flatpak run --command=flatpak-builder-lint org.flatpak.Builder appstream com.mateussouzaweb.NiceDeck.metainfo.xml

# Preview metainfo
gnome-software --show-metainfo com.mateussouzaweb.NiceDeck.metainfo.xml

# Build 
flatpak run org.flatpak.Builder --force-clean --sandbox --user --install --install-deps-from=flathub --ccache --mirror-screenshots-url=https://dl.flathub.org/media/ --repo=.flatpak-repo .flatpak-build ./com.mateussouzaweb.NiceDeck.yml

# Lint
flatpak run --command=flatpak-builder-lint org.flatpak.Builder builddir .flatpak-build
flatpak run --command=flatpak-builder-lint org.flatpak.Builder repo .flatpak-repo

# Run
flatpak run com.mateussouzaweb.NiceDeck
```

## Additional Notes

Required entry in ``exceptions.json`` at ``flatpak-builder-lint`` package:

```json
{
    "com.mateussouzaweb.NiceDeck": {
        "finish-args-flatpak-spawn-access": "the app predates this linter rule"
    }
}
```