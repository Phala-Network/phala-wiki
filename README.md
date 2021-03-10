# Phala Wiki Source

## Prepare

Init the repo:

```bash
git submodule update --init
```

(On macOS) Install [Hugo](https://gohugo.io/getting-started/installing/):

```bash
brew install hugo
```

(On Linux) Install:

```bash
sudo snap install hugo --channel=extended
```

## Live preview

```bash
hugo server
```

## Build & release

```bash
./scripts/publish.sh
```
