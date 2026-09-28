import fastf1
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation

fastf1.set_log_level("WARNING")

print("Loading Monaco GP...")

session = fastf1.get_session(
    2024,
    "Monaco Grand Prix",
    "R")

session.load(
    laps=True,
    telemetry=True,
    weather=False,
    messages=False)

print("Finding Leclerc's fastest lap...")

# New FastF1 syntax
CL16_laps = session.laps[
    session.laps["Driver"] == "LEC"]

pole_lap = CL16_laps.pick_fastest()

print("Lap:", int(pole_lap["LapNumber"]))
print("Lap time:", pole_lap["LapTime"])

# Get telemetry
telemetry = pole_lap.get_telemetry()

# Remove bad position/speed data
telemetry = telemetry.dropna(
    subset=["X", "Y", "Speed"])

# TELEMETRY DATA 

x = telemetry["X"].to_numpy()
y = telemetry["Y"].to_numpy()

speed = telemetry["Speed"].fillna(0).to_numpy()
throttle = telemetry["Throttle"].fillna(0).to_numpy()
brake = telemetry["Brake"].fillna(0).to_numpy()
gear = telemetry["nGear"].fillna(0).to_numpy()

distance = telemetry["Distance"].fillna(0).to_numpy()

plt.style.use("dark_background")

# Time from beginning of telemetry
time_seconds = (
    telemetry["SessionTime"]
    - telemetry["SessionTime"].iloc[0]
).dt.total_seconds().to_numpy()


# F1 TELEMETRY DASHBOARD
fig, ax = plt.subplots(figsize=(12, 7))

ax.set_title("F1 Telemetry Dashboard")
ax.set_xlabel("Track X Position")
ax.set_ylabel("Track Y Position")

line, = ax.plot([], [], linewidth=2)


def update(frame):

    live_time = time_seconds[frame]

    #car position
    line.set_data(
        x[:frame],
        y[:frame])

    ax.set_title(
        f"F1 Telemetry Dashboard | Time: {live_time:.2f} s")

    return line,


#ANIMATION
animation = FuncAnimation(
    fig,
    update,
    frames=len(x),
    interval=30,
    blit=False)

plt.show()

# UI COLORS

CYAN = "#00E5FF"
RED = "#FF304F"
WHITE = "#F5F7FA"
GREY = "#7D8590"
DARK = "#080b10"
PANEL = "#10151D"

fig = plt.figure(
    figsize=(16, 9),
    facecolor="#080b10")

# TRACK

ax_track = fig.add_axes(
    [0.04, 0.18, 0.57, 0.72],
    facecolor="#080b10")

# Track background
ax_track.plot(
    x,
    y,
    linewidth=15,
    alpha=0.5)

# Racing line
ax_track.plot(
    x,
    y,
    linewidth=4,
    color=CYAN,
    alpha=0.8)

# Car
car, = ax_track.plot(
    [],
    [],
    "o",
    markersize=13,
    color=RED,
    markeredgecolor=WHITE,
    markeredgewidth=1.5)

ax_track.set_aspect("equal")

ax_track.set_xlim(
    x.min() - 150,
    x.max() + 150)

ax_track.set_ylim(
    y.min() - 150,
    y.max() + 150)

# TITLE

fig.text(
    0.38,
    0.93,
    "F1 MONACO LAP SIMULATION",
    fontsize=30,
    fontweight="bold")

fig.text(
    0.19,
    0.850,
    "MONACO GRAND PRIX 2024",
    fontsize=14,
    alpha=0.7,
    fontweight="bold")

fig.text(
    0.075,
    0.77,
    "LECLERC",
    fontsize=22,
    fontweight="bold")

fig.text(
    0.077,
    0.70,
    "FERRARI  #16",
     color=RED,
    fontsize=11,
    alpha=0.7)

# SPEED

ax_speed = fig.add_axes(
    [0.66, 0.67, 0.30, 0.18],
    facecolor="#080b10")

ax_speed.plot(
    speed,
    linewidth=1.5)

speed_marker = ax_speed.axvline(
    0,
    linewidth=2,
    color=CYAN)

ax_speed.set_title(
    "SPEED",
    loc="left",
    fontsize=11)

ax_speed.set_ylabel("km/h")

ax_speed.set_xlim(
    0,
    len(speed))

ax_speed.set_ylim(0,max(speed) * 1.1)

ax_speed.grid(alpha=0.15)

# THROTTLE

ax_throttle = fig.add_axes(
    [0.66, 0.43, 0.30, 0.18],
    facecolor="#080b10")

ax_throttle.plot(
    throttle,
    linewidth=1.5)

throttle_marker = ax_throttle.axvline(
    0,
    linewidth=2,
    color=CYAN)

ax_throttle.set_title(
    "THROTTLE",
    loc="left",
    fontsize=11)

ax_throttle.set_ylabel("%")

ax_throttle.set_xlim(
    0,
    len(throttle))

ax_throttle.set_ylim(
    0,
    105)

ax_throttle.grid(
    alpha=0.15)

# BRAKE

ax_brake = fig.add_axes(
    [0.66, 0.19, 0.30, 0.18],
    facecolor="#080b10")

ax_brake.plot(
    brake,
    linewidth=1.5)

brake_marker = ax_brake.axvline(
    0,
    linewidth=2,
    color=RED)

ax_brake.set_title("BRAKE",loc="left",fontsize=11)

ax_brake.set_ylabel("%")

ax_brake.set_xlim(0,len(brake))

ax_brake.set_ylim(0,max(brake) * 1.1 + 1)

ax_brake.grid(alpha=0.15)

# LIVE DATA PANEL

fig.text(
    0.04,
    0.08,
    "LIVE DATA",
    fontsize=10,
    alpha=0.6)

gear_text = fig.text(
    0.14,
    0.065,
    "GEAR 1",
    fontsize=28,
    fontweight="bold")

speed_text = fig.text(
    0.30,
    0.065,
    "000 km/h",
    fontsize=28,
    fontweight="bold")

time_text = fig.text(
    0.52,
    0.065,
    "00.000",
    fontsize=24,
    fontweight="bold")

distance_text = fig.text(
    0.72,
    0.065,
    "0000 m",
    fontsize=18,
    alpha=0.8)

# LAP STATUS

lap_status = fig.text(
    0.04,
    0.105,
    "LAP 71  •  FASTEST LAP",
    fontsize=10,
    color=CYAN,
    fontweight="bold")

progress_text = fig.text(
    0.94,
    0.105,
    "0%",
    fontsize=10,
    color=WHITE,
    fontweight="bold",
    ha="right")

# BIG DRIVER DISPLAY

big_gear_text = fig.text(
    0.36,
    0.20,
    "1",
    fontsize=76,
    color=CYAN,)

big_gear_label = fig.text(
    0.36,
    0.14,
    "GEAR",
    fontsize=10,
    color=GREY,
    ha="center")

# ANIMATION

def update(frame):

    car.set_data(
        [x[frame]],
        [y[frame]]
    )

    speed_marker.set_xdata([frame, frame])

    throttle_marker.set_xdata([frame, frame])

    brake_marker.set_xdata([frame, frame])

    current_gear = int(round(gear[frame]))
    live_speed = speed[frame]
    live_time = time_seconds[frame]
    current_distance = distance[frame]

    big_gear_text.set_text(
    str(current_gear)
)

    gear_text.set_text(f"GEAR {current_gear}")

    speed_text.set_text(f"{live_speed:.0f} km/h" )

    time_text.set_text(f"{live_time:06.3f}")

    distance_text.set_text(f"{current_distance:.0f} m")

    return (
    car,
    speed_marker,
    throttle_marker,
    brake_marker,
    gear_text,
    speed_text,
    time_text,
    distance_text,
    big_gear_text
)

animation = FuncAnimation(
    fig,
    update,
    frames=len(x),
    interval=30,
    blit=False
)
plt.show()

time_seconds = (telemetry["SessionTime"]- telemetry["SessionTime"].iloc[0]).dt.total_seconds().to_numpy()

print("\n===== TELEMETRY VALUES =====")

print("\nBrake samples:")
print(telemetry["Brake"].head(20).to_string())

print("\nThrottle samples:")
print(telemetry["Throttle"].head(20).to_string())

print("\nGear samples:")
print(telemetry["nGear"].head(25).to_string())

print("\nBrake statistics:")
print(telemetry["Brake"].describe())

print("\nThrottle statistics:")
print(telemetry["Throttle"].describe())

print("\nGear statistics:")
print(telemetry["nGear"].describe())


# LAP ANALYSIS

Track_length = distance[-1]

average_speed = speed.mean()

top_speed = speed.max()


# F1 VEHICLE PHYSICS MODEL

import numpy as np

mass = 800
g = 9.8
downforce_coefficient = 3.0
drag_coefficient = 1.0
frontal_area = 1.5
air_density = 1.225
engine_power = 750000
coefficient_of_friction = 1.6

# FORWARD LAP SIMULATION

print("\n=======")
print("FORWARD SIMULATION")
print("=========")

# Simulation settings
dt_sim = 0.02          
sim_time = 0.0
sim_speed = 0.0        
sim_distance = 0.0

sim_times = []
sim_speeds = []
sim_distances = []

max_sim_time = 120

while sim_time < max_sim_time:

    # AERODYNAMIC FORCES
    
    downforce = (0.5* air_density* downforce_coefficient* frontal_area* sim_speed**2)

    drag = (0.5* air_density* drag_coefficient* frontal_area * sim_speed**2)

    # TYRE_GRIP

    normal_force = (mass * g + downforce)

    grip_limit = (coefficient_of_friction* normal_force)

    # ENGINE FORCE

    if sim_speed > 1:

        engine_force = (engine_power / sim_speed)

    else:

        engine_force = (engine_power / 1)

    engine_force = min(engine_force,grip_limit)

    # NET FORCE
 
    net_force = (engine_force - drag)

    # ACCELERATION

    acceleration_sim = (net_force/ mass)

    # UPDATE SPEED

    sim_speed += (acceleration_sim * dt_sim)

    # Prevent negative speed
    sim_speed = max(sim_speed,0)

    # UPDATE DISTANCE

    sim_distance += (sim_speed * dt_sim)

    sim_time += dt_sim

    # STORE DATA

    sim_times.append(sim_time)
    sim_speeds.append(sim_speed)
    sim_distances.append(sim_distance)

    # LAP COMPLETE

    if sim_distance >= Track_length:

        break

sim_lap_time = sim_time

print(f"Simulated lap time: "f"{sim_lap_time:.3f} s")

print(f"Simulated top speed: "f"{max(sim_speeds) * 3.6:.1f} km/h")

print(f"Simulated distance: " f"{sim_distance:.0f} m")

print("==============================")

# VEHICLE PARAMETERS

mass = 800                 
g = 9.81                   
# Aero
downforce_coefficient = 3.0
drag_coefficient = 1.0
frontal_area = 1.5         
air_density = 1.225        
# Engine
engine_power = 750000      
# Tyres
coefficient_of_friction = 1.6

# CONVERT SPEED
speed_ms = speed / 3.6

# AERODYNAMIC FORCES
downforce = (0.5* air_density* downforce_coefficient* frontal_area* speed_ms**2)

drag = (0.5* air_density* drag_coefficient* frontal_area* speed_ms**2)

# TYRE GRIP
normal_force = (mass * g+ downforce)

maximum_tyre_force = (coefficient_of_friction* normal_force)


# ENGINE FORCE

import numpy as np 

engine_force = np.zeros_like(speed_ms)

moving = speed_ms > 1

engine_force[moving] = (engine_power / speed_ms[moving])

# Engine cannot exceed tyre grip
engine_force = np.minimum(engine_force,maximum_tyre_force)

# NET FORCE
force = (engine_force - drag)

# ACCELERATION
acceleration = ( force / mass)

# DISPLAY

print("\n======================")
print("F1 PHYSICS MODEL")
print("========================")

print(f"Maximum downforce: "f"{downforce.max():.0f} N")

print(f"Maximum drag:      "f"{drag.max():.0f} N")

print(f"Maximum tyre force:" f" {maximum_tyre_force.max():.0f} N")

print(f"Peak acceleration:"f" {acceleration.max():.2f} m/s²")

print(f"Peak acceleration:"f" {acceleration.max()/g:.2f} G")

# CALCULATE TIME INTERVALS

dt = time_seconds[1:] - time_seconds[:-1]

# BRAKING TIME

braking = brake[:-1] > 5

total_braking_time = dt[braking].sum()

# FULL THROTTLE TIME

full_throttle = throttle[:-1] > 95

total_full_throttle_time = dt[full_throttle].sum()

# LAP TIME
lap_time = time_seconds[-1]

# RESULTS
print("\n===================")
print("LAP ANALYSIS")
print("===================")

print(f"Track length: {Track_length:.0f} m")

print(f"Average speed: {average_speed:.1f} km/h")

print(f"Top speed: {top_speed:.1f} km/h")

print(f"Braking time: {total_braking_time:.2f} s")

print(f"Full throttle time: {total_full_throttle_time:.2f} s")

print(f"Lap time: {lap_time:.3f} s")
