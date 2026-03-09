name: Build AMLL Player Linux ARM64

on:
  workflow_dispatch:

jobs:
  build-linux-arm64:
    runs-on: ubuntu-22.04

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: 安装 Rust
        uses: dtolnay/rust-toolchain@stable

      - name: 安装 Node
        uses: actions/setup-node@v4
        with:
          node-version: 18

      - name: 安装依赖
        run: |
          sudo apt update
          sudo apt install -y \
          libgtk-3-dev \
          libwebkit2gtk-4.1-dev \
          libappindicator3-dev \
          librsvg2-dev \
          patchelf

      - name: 安装 wasm-pack
        uses: jetli/wasm-pack-action@v0.4.0
        with:
          version: latest

      - name: 安装 wasm32 目标
        run: rustup target add wasm32-unknown-unknown

      - name: 构建 AMLL
        run: |
          yarn
          yarn lerna run build:dev --scope "@applemusic-like-lyrics/*"
        env:
          YARN_ENABLE_IMMUTABLE_INSTALLS: false
          AMLL_GITHUB_IS_ACTION: true

      - name: 构建 ARM64 Tauri
        run: |
          cd packages/player
          cargo tauri build --target aarch64-unknown-linux-gnu

      - name: 上传 ARM64
        uses: actions/upload-artifact@v4
        with:
          name: amll-linux-arm64
          path: |
            packages/player/src-tauri/target/aarch64-unknown-linux-gnu/release/bundle/**/*.AppImage
            packages/player/src-tauri/target/aarch64-unknown-linux-gnu/release/bundle/**/*.deb
