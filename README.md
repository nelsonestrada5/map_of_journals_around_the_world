# Proyecto: Mapa Global de Ventas de Journals
<aside>
⭐

**META:** Crear un mapa del mundo que muestre nuestra presencia global a través de las ventas de Journals.

</aside>

## Tareas a Alto Nivel

- Encontrar una herramienta de visualización geográfica adecuada.
- Crear un archivo CSV con las coordenadas (latitud y longitud) extraídas de las direcciones de entrega.
- Subir el CSV a la herramienta de visualización.
- Integrar el mapa en el sitio web de Kajabi.

## 1. Posibles Herramientas de Visualización

- CesiumJS
    - **Pros:** Permite crear pines en un entorno 3D.
    - **Contras:** Cada pin se debe agregar manualmente en la interfaz web. Para manejar miles de coordenadas, se requeriría desarrollar una aplicación web (por ejemplo, con Node.js), lo que resulta más complejo y lleva más tiempo.
- ArcGIS
    - **Pros:** Es una opción interesante y de código abierto.
    - **Contras:** La estética no es tan atractiva, por lo que podría no funcionar bien para nuestra presentación.
- [Kepler.gl](http://kepler.gl/)
    - **Pros:** Visualización muy estética y fácil de usar. Cumple con los requerimientos del proyecto.
    - **Contras:** Es un mapa 2D, pero eso es aceptable para nuestros fines.
    - **Decisión:** Se eligió [Kepler.gl](http://kepler.gl/) porque es la solución que mejor equilibra estética y funcionalidad.
- Three.js
    - **Contras:** Es aún más complejo de implementar, ya que requiere crear un modelo 3D del mundo desde cero.

## 2. Herramienta de Geocodificación

- Se utilizó la librería de Python `geopy` para obtener las coordenadas a partir de cada ciudad y país.
- Se ejecutó el script en paralelo usando el comando `screen` para acelerar el proceso.

```python
# Ejemplo de script usando geopy
from geopy.geocoders import Nominatim
import csv

geolocator = Nominatim(user_agent="journal_map")
with open('locations.csv', 'r') as infile, open('coordinates.csv', 'w', newline='') as outfile:
    reader = csv.DictReader(infile)
    fieldnames = reader.fieldnames + ['latitude', 'longitude']
    writer = csv.DictWriter(outfile, fieldnames=fieldnames)
    writer.writeheader()
    for row in reader:
        location = geolocator.geocode(f"{row['City']}, {row['Country']}")
        row['latitude'] = location.latitude if location else ''
        row['longitude'] = location.longitude if location else ''
        writer.writerow(row)
```

## **3. Subida del Archivo a Kepler.gl**

- Se cargó el archivo CSV en Kepler.gl para probar la visualización.
- Se verificó que las coordenadas se representaran correctamente en el mapa.

## **4. Exportar el Mapa y Publicarlo en GitHub Pages**

- Se exportó el mapa en formato HTML desde Kepler.gl.
- **Importante:** Se debe tener un token de Mapbox válido para la exportación, de modo que la visualización no expire.
    - Puedes obtener un token en: [Mapbox Tokens](https://console.mapbox.com/account/access-tokens)
- Se creó el repositorio en GitHub: [map_of_journals_around_the_world](https://github.com/nelsonestrada5/map_of_journals_around_the_world).
- Se agregó el archivo index.html directamente en GitHub.
- Se habilitó GitHub Pages para el repositorio:
    1. En el repositorio, ir a **Settings**.
    2. Buscar la sección **Pages**.
    3. Seleccionar la rama main y la carpeta raíz (/).
    4. GitHub generará una URL pública, por ejemplo:
        
        https://nelsonestrada5.github.io/map_of_journals_around_the_world/
        

## **5. Integración en Kajabi**

- Se integró el mapa en Kajabi usando un iframe.
- Se añadió un bloque de código personalizado en la página de Kajabi con el siguiente código:

```html
<iframe src="https://nelsonestrada5.github.io/map_of_journals_around_the_world/" 
        width="100%" 
        height="600" 
        style="border: none;">
</iframe>
```
<img width="2672" alt="image" src="https://github.com/user-attachments/assets/ba0bfe59-e947-4065-826e-00e8867c59fc" />


- Se ajustaron las dimensiones según las necesidades del diseño.
