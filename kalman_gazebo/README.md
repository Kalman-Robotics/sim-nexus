# kalman_gazebo package 

- [kalman\_gazebo package](#kalman_gazebo-package)
  - [Archivos Launch](#archivos-launch)
    - [`simulation.launch.py`](#simulationlaunchpy)
    - [`self_drive_gazebo.launch.py`](#self_drive_gazebolaunchpy)
  - [Uso](#uso)
  - [Aplicaciones](#aplicaciones)
    - [Comando por Teclado](#comando-por-teclado)
    - [Navegación Autónoma](#navegación-autónoma)

## Archivos Launch

### `simulation.launch.py`
Lanza la simulación de Gazebo con el robot Kalman.

**Argumentos:**
- `use_sim_time`: Usar reloj de simulación (Gazebo) si es verdadero (default: `true`)
  - Valores aceptados: `true`, `false`
- `robot_model`: Nombre del paquete de descripción del robot (default: vacío, usa configuración)
- `x_pose`: Posición inicial X del robot (default: vacío, usa el punto libre del mundo elegido)
- `y_pose`: Posición inicial Y del robot (default: vacío, usa el punto libre del mundo elegido)
- `world`: Nombre del archivo de mundo (default: `laboratorio_real.world`, la pista real)

Cada mundo tiene su zona despejada, así que el punto de arranque sale de la tabla
`SPAWN_POR_MUNDO` del propio launch: `laboratorio_real.world` arranca en `(0.0, 0.0)`,
el centro del recinto de 1,45 × 1,55 m (0,25 m de holgura alrededor del robot), y
`laboratorio.world` en `(-0.15, -0.89)`, una calle al este de la ciudad. Pasar
`x_pose`/`y_pose` a mano tiene prioridad sobre esa tabla.

### `self_drive_gazebo.launch.py`
Lanza el nodo de evitación de obstáculos simple para Gazebo. Se suscribe a LaserScan y Odometry (yaw), publica velocidad. Cada 10 ms elige un estado (avanzar, girar izquierda, girar derecha) según los umbrales "check.angle" y "check.distance" y publica las velocidades correspondientes. 

**Argumentos:**
- `robot_model`: Nombre del paquete de descripción del robot (default: vacío, usa configuración)

## Uso
Para lanzar la simulación de Gazebo con el robot Kalman, use el siguiente comando:
```
ros2 launch kalman_gazebo simulation.launch.py robot_model:=kalman_description world:=laboratorio_real.world
```

## Aplicaciones
Para ejecutar las utilidades de los paquetes de Kalman, asegurese de configurar el argumento `use_sim_time` a `true` para sincronizar con el tiempo simulado de Gazebo.

### Comando por Teclado
Para conducir el robot con el teclado, en **otra terminal** (con la simulación ya
corriendo):
```
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```
Publica en `/cmd_vel`, que es el tópico que escucha el robot. La ventana de la
terminal donde lo ejecutes tiene que estar enfocada para que registre las teclas.

### Conducción autónoma de ejemplo
Evitación de obstáculos simple, incluida en este paquete: lee `/scan`, elige entre
avanzar o girar y publica en `/cmd_vel`.
```
ros2 launch kalman_gazebo self_drive_gazebo.launch.py
```

> **Nota:** la navegación completa (Nav2, SLAM y mapas) vive en `kalman_bringup`, que
> es parte del kit del robot real y **no está incluido en este repositorio de
> simulación**.
