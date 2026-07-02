# tcxOpenCV

OpenCV integration addon for [TrussC](https://trussc.org).

This addon provides minimal glue between TrussC and OpenCV - type conversion utilities that let you use OpenCV's powerful image processing with TrussC's rendering.

## Installation

Clone this repository into your TrussC `addons/` folder:

```bash
cd path/to/TrussC/addons
git clone https://github.com/TrussC-org/tcxOpenCV.git
```

Then add `tcxOpenCV` to your project's `addons.make`:

```
tcxOpenCV
```

That's it! OpenCV 4.10.0 will be automatically fetched and built on first compile.

## Usage

```cpp
#include <TrussC.h>
#include <tcxOpenCV.h>

using namespace std;
using namespace tc;

class tcApp : public App {
    Image img;
    Image processed;

    void setup() override {
        img.load("photo.jpg");
    }

    void update() override {
        // Convert tc::Image to cv::Mat
        cv::Mat mat = tcx::opencv::toCvMat(img);

        // Use OpenCV as usual
        cv::Mat blurred;
        cv::GaussianBlur(mat, blurred, cv::Size(15, 15), 0);

        // Convert back to tc::Image
        tcx::opencv::toTcImage(blurred, processed);
    }

    void draw() override {
        processed.draw(0, 0);
    }
};
```

## API Reference

Conversion helpers live in the canonical namespace **`tcx::opencv`**.

> The flat `tcx::` spelling (e.g. `tcx::toCvMat`) still compiles via silent
> aliases, but it is **deprecated** and will be removed at **v1.0.0**. Prefer
> `tcx::opencv::`.

### Image Conversion

| Function | Description |
|----------|-------------|
| `tcx::opencv::toCvMat(tc::Image&)` | Convert tc::Image to cv::Mat (BGR) |
| `tcx::opencv::toCvMatRGBA(tc::Image&)` | Convert tc::Image to cv::Mat (RGBA) |
| `tcx::opencv::toCvMat(tc::VideoGrabber&)` | Convert VideoGrabber frame to cv::Mat |
| `tcx::opencv::toTcImage(cv::Mat&)` | Convert cv::Mat to new tc::Image |
| `tcx::opencv::toTcImage(cv::Mat&, tc::Image&)` | Convert cv::Mat into existing tc::Image |

### Type Conversion

| Function | Description |
|----------|-------------|
| `tcx::opencv::toCvScalar(tc::Color)` | tc::Color (RGBA 0-1) to cv::Scalar (BGR 0-255) |
| `tcx::opencv::toTcColor(cv::Scalar)` | cv::Scalar to tc::Color |
| `tcx::opencv::toCvPoint(tc::Vec2)` | tc::Vec2 to cv::Point2f |
| `tcx::opencv::toCvPointI(tc::Vec2)` | tc::Vec2 to cv::Point (integer) |
| `tcx::opencv::toTcVec2(cv::Point2f)` | cv::Point2f to tc::Vec2 |
| `tcx::opencv::toTcVec2(cv::Point)` | cv::Point to tc::Vec2 |
| `tcx::opencv::toCvRect(tc::Rect)` | tc::Rect to cv::Rect |
| `tcx::opencv::toTcRect(cv::Rect)` | cv::Rect to tc::Rect |

### Contour Helper

| Function | Description |
|----------|-------------|
| `tcx::opencv::toTcContours(vector<vector<cv::Point>>&)` | Convert cv contours to `vector<vector<tc::Vec2>>` |

## Design Philosophy

This addon provides **minimal glue**, not a full wrapper:

- Use `cv::` directly for image processing - OpenCV has extensive documentation
- Only type conversion is wrapped in `tcx::opencv::`
- TrussC types (Image, Color, Vec2) work seamlessly with OpenCV

## OpenCV Modules Included

- `core` - Basic structures
- `imgproc` - Image processing (blur, edge detection, etc.)
- `imgcodecs` - Image file I/O
- `videoio` - Video capture
- `objdetect` - Object detection (including ArUco markers)
- `features2d` - Feature detection
- `calib3d` - Camera calibration, 3D reconstruction

## Requirements

- TrussC (https://github.com/TrussC-org/TrussC)
- CMake 3.20+
- C++20 compiler

## License

MIT License - see [LICENSES.md](LICENSES.md)

This addon integrates [OpenCV](https://opencv.org/), which is licensed under Apache 2.0 License.

## Links

- TrussC: https://trussc.org
- TrussC GitHub: https://github.com/TrussC-org/TrussC
- OpenCV: https://opencv.org
