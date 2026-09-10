# 🛠️ Lavender Kernel Build CI

This GitHub Actions workflow automatically compiles the custom Android kernel for the Xiaomi Redmi Note 7 (`lavender`), packages it into an AnyKernel3 flashable ZIP, and sends real-time build notifications straight to Telegram.

---

## 🚀 Key Inputs & Customization
When triggering this workflow manually from the **Actions** tab, you can configure the following options:
* **👤 Telegram Username:** Your Telegram handle to properly credit the author of the build.
* **🏷️ Custom Kernel Name (`LOCALVERSION`):** Define a custom custom suffix string for your kernel name (e.g., `MyKernel-v1.0`).
* **🔧 Localversion Auto:** Toggle whether to append git/automatic version info.
* **📳 QTI Haptics:** Enable or disable support for advanced vibration drivers.
* **📷 Build Newcam:** Switch between old camera blobs or Xiaomi's new camera blobs build.
* **🦊 Inject KernelSU:** Choose whether to integrate KernelSU variants (`kowsu`, `xxksu`, or leave it as `none`).

---

## ⚙️ What the Workflow Does

1. **Source Synchronization:** Clones the specific kernel source repository (`ExXyrou/android_ps_kernel_xiaomi_lavender`) on the `oldcam` branch.
2. **Environment & Toolchain Setup:** 
   * Installs necessary compilation packages and build essentials.
   * Sets up **ccache** to speed up rebuild times.
   * Downloads and caches **AOSP Clang (r383902)** for compiling.
3. **Kernel Configuration:** Applies the device-specific performance defconfig (`lavender-perf_defconfig`), injects KernelSU if requested, and configures the version string and hostnames (`xiaomi@redmi`).
4. **Compilation:** Compiles the kernel using all available CPU threads (`nproc --all`) with optimized flags, generating the target `Image.gz-dtb` file and tracking output logs (`build.log`).
5. **Packaging:** Clones **AnyKernel3**, bundles the compiled kernel image, updates device properties for `lavender`, and compresses everything into a flashable `.zip` package.
6. **Telegram Integration:** 
   * **On Success:** Automatically uploads the flashable `.zip` file directly to the configured Telegram group with a detailed summary caption.
   * **On Failure:** Captures and uploads the `build.log` file so you can easily trace and debug the error.
