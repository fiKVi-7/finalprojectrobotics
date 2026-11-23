# Line Follower Report Package — read me first

Everything you need to write and submit the Final Group Project report, in one place.
Project: line follower robot with IR sensors, simulated in ROS 2 Humble + Gazebo Fortress.

---

## 1. What is in this package

| Path | What it is | Use it for |
|---|---|---|
| `report.docx` | Full report, Word format, figures embedded | **Start here** if your campus wants Word: fill in the gaps, apply your template |
| `report.pdf` | Same report as PDF (A4) | Quick reading / submitting as-is after filling in |
| `report.md` | Same report as Markdown (source text) | Copy-paste text, view on GitHub/VS Code |
| `figures/` | 9 PNG images (charts, diagrams, screenshots) | Insert into slides or your own template |
| `diagrams/` | Graphviz sources of the 2 block diagrams | Edit the diagrams, then `dot -Tpng hardware.dot -o hardware.png` |
| `video/simulasi_line_follower.mp4` | Simulation video, H.264, 1280×712, 38.28 s | Submit / embed in slides (plays on Windows, Mac, browsers, phones) |
| `video/simulasi_line_follower.webm` | Same video, WebM/VP8 | Plays on the Ubuntu VM without extra codecs |
| `data/results.csv` | Summary of the 5 experiments (the source of every number in the report) | Tables, checking numbers |
| `data/results_odom_only.csv` | Early exploratory runs judged with wheel odometry | Only for §7.4 (origin of kp = 3) and §7.5 (odometry problem) |
| `data/experiments/<run>/` | Per-run raw data: CSV log (10 rows/s), lap CSV, `params.yaml`, `launch.log` | Appendix, re-plotting, proof |
| `data/evidence/` | The two extra lap files cited in the report (§7.5 and §7.6) | Proof for those two statements |
| `code/line_follower/` | Complete ROS 2 package source (nodes, launch, model, world, config, scripts) | Code appendix, listings |
| `archive/laporan_id.md` | Older Indonesian draft of the report | Reference only — **not** up to date |

---

## 2. Checklist: what the group still has to do

- [x] **Title block**: members + student IDs, course and lecturer are filled in (add a group name and date if required).
- [ ] **Task division** (if your campus format asks for it): who did what.
- [ ] **Conclusion / reflection** in your own words (if required). A factual conclusion is already in §11.
- [ ] **Hardware section (§3)**: add prices or a circuit schematic only if your lecturer asks. Everything in §3 is a
      *proposal* and untested — keep that wording.
- [ ] **Apply the campus template** (fonts, cover page, numbering) in `report.docx`.
- [ ] **Read the whole report once** and make sure you can explain every section (it is your submission).
- [ ] Optional: if you present slides, use the figures in `figures/` and the video.

---

## 3. Key numbers (quick reference)

Every number below is copied from `data/results.csv` unless another file is named. Durations are
**simulation time**.

| Run | Lap time (s) | Mean \|error\| | Max \|error\| | Line lost | Max deviation (cm) |
|---|---|---|---|---|---|
| 0.15 m/s, kp = 1 (baseline) | 95.30 | 0.0662 | 0.1296 | 0 | 0.86 |
| 0.25 m/s, kp = 1 | 59.87 | 0.1048 | 0.2200 | 0 | 0.77 |
| 0.35 m/s, kp = 1 | 44.71 | 0.1411 | 0.2500 | 0 | 0.72 |
| 0.45 m/s, kp = 1 | 36.31 | 0.1735 | 0.3333 | 0 | 0.59 |
| **0.45 m/s, kp = 3 (best)** | **32.47** | **0.0776** | **0.1923** | **0** | **0.86** |

Other facts used in the report:

| Fact | Value | Source |
|---|---|---|
| Track length | 13.54 m (2 × 3 m straights + 2 half circles R = 1.2 m) | `code/line_follower/scripts/generate_track.py` |
| Robot | 20 × 12 × 5 cm, 0.8 kg, wheel radius 3.5 cm, wheel separation 15 cm | `code/line_follower/models/line_robot/model.sdf` |
| Sensors | 5 virtual IR sensors, 1.5 cm apart, 16 cm ahead of the axle, camera 40 × 4 px | `model.sdf`, `launch/sim.launch.py` |
| Curve error theory vs measured | e = a/(1+a), a = base_speed/(R·kp); match within 0.004 | `data/results.csv` column `mean_error_in_curves` |
| Steering sign flips per lap | 0 with kp = 1, 14 with kp = 3 | `data/results.csv` column `steering_sign_flips` |
| kd makes it worse (exploratory) | kp = 3: max \|error\| 0.7500 (kd 0.05), 0.9286 (kd 0.1); kp = 4, kd = 0.1 failed | `data/results_odom_only.csv` |
| Odometry drift example | 25.90 m recorded for one "lap" at 2.5 m/s | `data/evidence/odom_slip_v2.50_laps.csv` |
| Video run lap time | 32.27 s (separate demo run, not one of the 5 experiments) | `data/evidence/video_run_laps.csv` |

Rules to keep the report honest:
- Do not add numbers that are not in these files.
- Say that the IR sensor is **simulated with a camera** (§2.3) and that evaluation uses **ground truth** because
  wheel odometry slips (§7.5).
- Say that only **5 experiments** were run and the **maximum speed was not determined** (§7.1, §8).

---

## 4. Which figure goes where

| Report section | Figure file | Suggested caption |
|---|---|---|
| §2.1 Robot design | `figures/robot_closeup.png` | Robot on the track (close-up, Gazebo) |
| §2.1 / §6 Setup | `figures/gazebo_overview.png` | Whole track with the robot at the start |
| §3 Hardware | `figures/diagram_hardware.png` | Proposed hardware block diagram |
| §4 Software | `figures/diagram_software.png` | ROS 2 nodes and topics |
| §7.3 Results | `figures/lap_time_vs_speed.png` | Lap time per run |
| §7.3 Results | `figures/error_vs_time.png` | Line position error vs time |
| §7.3 Results | `figures/speed_vs_time.png` | Forward speed command vs time |
| §7.3 Results | `figures/trajectory.png` | Robot path (ground truth) on the track |
| §7.3 Results | `figures/gain_comparison.png` | kp = 1 vs kp = 3 at 0.45 m/s |
| §7.6 Video | `video/simulasi_line_follower.mp4` | One lap at 0.45 m/s, kp = 3 |

---

## 5. Report outline (sections already written)

1. Objective and scope
2. Robot design: dimensions, sensor placement, why a camera simulates the IR sensor
3. Real-hardware equivalent: TCRT5000, ADC/LM393, Arduino Nano/ESP32, TB6612FNG, block diagram
4. Software architecture: nodes, topics, diagram
5. Control algorithm: weighted average, PID, line-lost search, curve-error analysis
6. Simulation setup: track, physics, rendering, experiment procedure, metric definitions
7. Experimental results: run list, results table, figures, origin of kp = 3, odometry note, video
8. Limitations
9. How to reproduce
10. Parts the group still needs to fill in
11. Conclusion

---

## 6. Practical notes

- **Video does not play on the Ubuntu VM?** Use the `.webm`, or install the decoder once:
  `sudo apt install gstreamer1.0-libav`. The `.mp4` plays everywhere else.
- **Regenerating things** (from `~/line_follower_ws`, after `source install/setup.bash`):
  - figures: `python3 src/line_follower/scripts/make_figures.py`
  - Word/PDF from `report.md`: `python3 src/line_follower/scripts/export_report.py`
  - experiments (about 5 min): `python3 src/line_follower/scripts/run_experiments.py --speeds 0.15 0.25 0.35 0.45 --runs 0.45,3.0,0.0`
- **Run the simulation live** (for a demo): `ros2 launch line_follower sim.launch.py follow:=true`
- The originals live in `~/line_follower_ws/src/line_follower/docs/`; this package is a snapshot. If you edit the
  report, edit `report.docx` here (or `docs/report.md` and re-export) — not both.
