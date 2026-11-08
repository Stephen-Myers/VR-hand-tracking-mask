# Running hand segmentation

From Git Bash, start in the project folder and use the virtual environment's Python. Plain `python` on this machine is Python 3.14 and will not run the script.

```bash
cd ~/Homework/CS5680/Project/lightweight-hand-segmentation
./.venv/Scripts/python.exe hand_segmentation.py
```

The `./` is required. Without it, Bash uses `C:\Python314\python.exe` instead of the environment in `.venv`.

Images are read from `Ego2Hands_eval/eval_seq8_imgs/`. The first window is slow because each frame is processed 100 times. Press any key to advance to the next frame.
