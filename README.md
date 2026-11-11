# SmartShield

ROS 2 alapú projekt az Autonóm járművek és robotok programozása tantárgy nagybeadandójához.

## Jelenlegi működés

A kisbeadandóból átvett, tesztelt alap két node-ból áll:

- `virtual_lidar`: virtuális LIDAR-méréseket publikál a `/scan` topicra.
- `obstacle_detector`: a robot előtti mérésekből meghatározza a legközelebbi akadály távolságát, és kiírja az állapotot.

| Távolság | Állapot |
| --- | --- |
| 1,0 m alatt | AKADÁLY |
| 1,0 m-től 2,0 m alatt | FIGYELEM |
| 2,0 m-től | SZABAD |

## Környezet

Windows 10, WSL2, Ubuntu 26.04, ROS 2 Lyrical, Python 3 és colcon.

## Fordítás

```bash
source /opt/ros/lyrical/setup.bash
cd ~/smartshield_ws
colcon build --symlink-install --packages-select virtual_obstacle_detector
source install/setup.bash
```

## Indítás

Az első WSL terminálban:

```bash
source /opt/ros/lyrical/setup.bash
source ~/smartshield_ws/install/setup.bash
ros2 run virtual_obstacle_detector obstacle_detector
```

A második WSL terminálban:

```bash
source /opt/ros/lyrical/setup.bash
source ~/smartshield_ws/install/setup.bash
ros2 run virtual_obstacle_detector virtual_lidar --ros-args -p obstacle_distance:=0.5
```

A LIDAR leállítása: Ctrl+C. Újraindításkor az `obstacle_distance` értéke módosítható.

## Elvégzett működési tesztek

| Beállított távolság | Megfigyelt állapot | Eredmény |
| --- | --- | --- |
| 0,5 m | AKADÁLY | Sikeres |
| 1,5 m | FIGYELEM | Sikeres |
| 5,0 m | SZABAD | Sikeres |

A teszteket a másolt csomaggal, a SmartShield munkaterületén végeztük el.

## Eredeti projekt

A kiinduló kisbeadandó: https://github.com/Cristmax/mol_pw1_lidar
