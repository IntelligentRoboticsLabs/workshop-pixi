# Nav2 completo con `pixi-ros`, simulación y navegación

Este ejercicio no tiene carpeta `solucion/` porque el `pixi.toml` se **genera automáticamente** por `pixi-ros init` a partir de los `package.xml` reales de Nav2 — depende de la versión exacta de Nav2 en el momento de clonar, así que no tiene sentido congelarlo aquí como referencia estática.

## Comandos, en orden

```bash
# 1. Instalar la herramienta (una vez)
pixi global install pixi-ros

# 2. Crear el workspace y clonar Nav2
mkdir -p nav2_ws/src && cd nav2_ws
git clone -b jazzy https://github.com/ros-navigation/navigation2.git src/navigation2

# 3. Rellenar dependencias automáticamente
pixi-ros init --distro jazzy --platform linux-64

# 4. Ajustar versiones de dependencias
pixi add gcc_linux-64=13.3.0 gxx_linux-64=13.3.0
pixi add libboost=1.88.0 libboost-devel=1.88.0
pixi add ros-jazzy-rviz2

# 5. Instalar y compilar
pixi install
pixi run build

# 6. Verificar
pixi shell
ros2 pkg list | grep nav2
ros2 pkg executables nav2_costmap_2d

# 7. Simulación + RViz2
ros2 launch nav2_bringup tb3_simulation_launch.py
```

En RViz2: **2D Pose Estimate** (localizar el robot) → **Nav2 Goal** (marcar destino) → el robot navega solo.

## Troubleshooting

- **`gz sim` no abre ventana / va muy lento**: necesita una pila gráfica (OpenGL) funcional. En VMs o WSL2 sin passthrough de GPU, prueba:
  ```bash
  LIBGL_ALWAYS_SOFTWARE=1 ros2 launch nav2_bringup tb3_simulation_launch.py
  ```
- **`pixi-ros init` no encuentra alguna dependencia** (aparece como `NOT FOUND` en la tabla de validación): revisa que el nombre del paquete en `package.xml` coincida con el índice de la distro ROS; en último caso se puede añadir un canal extra con `--channel`.
- **El build tarda mucho**: es esperable, Nav2 son más de 80 paquetes C++ — el instructor suele lanzarlo con antelación y mostrar el resultado en vivo.

## Referencia

- Herramienta: <https://github.com/ruben-arts/pixi-ros>
- Repo de Nav2: <https://github.com/ros-navigation/navigation2> (rama `jazzy`)
