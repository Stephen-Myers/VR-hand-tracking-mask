# Running hand segmentation

The script reads still frames from Ego2Hands and opens the segmentation in several windows. It uses the old MediaPipe Hands API (`mp.solutions.hands`), which was removed in MediaPipe 0.10.30. Use Python 3.11 and `mediapipe==0.10.21`. The default `python` on this machine is Python 3.14 and cannot run the script.

## 1. Environment

From the project folder in PowerShell:

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\python.exe -m pip install --upgrade pip
.\.venv\Scripts\python.exe -m pip install mediapipe==0.10.21 opencv-python numpy tqdm scikit-learn matplotlib
```

`py -3.11` is the Windows Python launcher. There is no `python3.11` command on PATH. Use `opencv-python`, not the headless package, because the script opens windows with `cv2.imshow`.

A `.venv` for this machine is already created and is not part of the git repository. After a fresh clone, run the commands above before the run step.

## 2. Ego2Hands images

Download only the evaluation set. The training set is about 90 GB, and this script never uses it.

https://app.box.com/s/gd1uywmyeodpwcyyi3dnyfrb8oybe8nx

Unzipping that download produces four archives, `eval_0.tar` through `eval_3.tar`, not image folders. Sequence 8, the only sequence the script reads, is inside `eval_3.tar`. From the project folder:

```powershell
New-Item -ItemType Directory -Force -Path .\Ego2Hands_eval | Out-Null
tar -xf .\eval\eval_3.tar -C .\Ego2Hands_eval eval_seq8_imgs
```

Frames and masks then sit here:

```text
Ego2Hands_eval\eval_seq8_imgs\00000.png
Ego2Hands_eval\eval_seq8_imgs\00000_seg.png
```

The script keeps only filenames that are exactly 9 characters, such as `00000.png`, and loads the mask by adding `_seg.png`. The archives and the unpacked frames are listed in `.gitignore`. Ego2Hands is for scientific and non-commercial use.

## 3. Run it

From Git Bash, in the project folder:

```bash
cd ~/Homework/CS5680/Project/lightweight-hand-segmentation
./.venv/Scripts/python.exe hand_segmentation.py
```

The `./` is required. Without it, Bash uses `C:\Python314\python.exe` instead of the environment in `.venv`.

The first window is slow because each frame is segmented 100 times before anything is drawn. Press any key to move to the next frame. `Ctrl+C` in the terminal stops the run. Each frame overwrites `test.png`, `test_mp.png`, and `test_mask.png` in the project folder.

## Goals

- Add a live camera mode and measure frames per second against the paper's claim of about 90.
- Show the mask as an occlusion overlay on that video.
- Change the edge-cleanup kernels from rectangles to another shape and compare the results.
- Test the same program in dark, normal, and bright lighting.
