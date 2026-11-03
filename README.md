# 🥋 Poomsae Coach

**An AI coach that analyzes Taekwondo forms (poomsae) from a phone video and tells students exactly what to fix.**

> 🚧 **Status: in progress.** Started November 2026. See the [roadmap](#roadmap).

## Why

I run a small Taekwondo school in Tuxtla Gutiérrez, Mexico. Students practice their forms at home, but between classes nobody can tell them that their front stance is too short or their low block is too high. Poomsae Coach lets them record themselves with a phone and get feedback on every movement.

## How it works

```
phone video ──► pose estimation ──► normalization ──► joint angles ──► DTW alignment ──► per-move feedback
               (MediaPipe, 33        (center on hips,   (elbow, knee,    (vs. a reference
                keypoints/frame)      scale by torso)    hip, shoulder)   performance)
```

1. **Pose estimation:** MediaPipe Pose extracts 33 body keypoints per frame.
2. **Normalization:** skeletons are centered on the hips and scaled by torso length, so height and camera distance don't matter.
3. **Features:** joint angles (elbows, knees, hips, shoulders) per frame.
4. **Alignment:** students perform at different speeds, so the sequence is aligned with a reference performance using **Dynamic Time Warping, implemented from scratch** (dynamic programming, O(n·m)).
5. **Feedback:** the form is segmented into movements (detected by pauses in velocity), and each movement is scored with angle thresholds: *"Move 7, low block: arm 25° too high."*

## Privacy by design

Most users are minors. **Videos are processed and then discarded**; only anonymous skeleton keypoints (numbers, no faces) are stored. Parental consent is collected through the school's platform. All public demos use my own videos.

## Tech stack

Python · MediaPipe · NumPy · OpenCV · FastAPI. Results are integrated into [Dojang](https://dojang.vercel.app), the management platform I built for my school.

## Roadmap

- [ ] Read a video and draw the detected skeleton on each frame
- [ ] Record a reference performance (Taegeuk Il Jang); normalization and joint angles
- [ ] Dynamic Time Warping from scratch + overall score
- [ ] Movement segmentation + per-move text feedback
- [ ] FastAPI endpoint + upload screen in Dojang
- [ ] Pilot with students; publish metrics and a side-by-side demo GIF

## Author

**Ricardo Pinto**, Computer Systems Engineering student at ITTG, Mexico · [GitHub](https://github.com/RicardoPVX)
