# 💡 LABENERGY — Simulador Interactivo de Eficiencia Energética

Aplicación web interactiva diseñada para demostrar el ahorro energético de un sistema de iluminación inteligente controlado mediante un **sensor LDR** y un microcontrolador **ESP32** con modulación por ancho de pulso (**PWM**), comparándolo frente a una luminaria tradicional que opera de forma continua al 100%.

🔗 **Repositorio:** [ColthWhite/proyecto-de-inovacion-tecnologico](https://github.com/ColthWhite/proyecto-de-inovacion-tecnologico)  
🎨 **Prototipo / Diseño:** [Ver en Figma](https://www.figma.com/make/nGm7jXQCee7gvks3wuqJD7/Review-Attached-Content?t=nnpu7ZJYl9uxcq0e-1)

---

## ✨ Características Principales

* **Simulación Visual en Tiempo Real (`RoomScene`):** Escena SVG dinámica de una habitación con ventana y lámpara que ajusta la iluminación ambiental, la intensidad solar y el halo de la luminaria según la hora del día y la nubosidad.
* **Panel de Control Interactivo:** Permite modificar en vivo tres parámetros clave[cite: 5]:
  * **Hora del día:** De `00:00` a `24:00` (con opción de reproducción automática del ciclo diario)[cite: 5].
  * **Nubosidad:** De `0%` a `100%`[cite: 5].
  * **Potencia máxima de la luminaria:** De `3 W` a `20 W` (valor base: `9 W`)[cite: 5].
* **Telemetría del Sensor:** Visualización instantánea del porcentaje de luz ambiente detectada, valor **PWM** (`0–255`) y consumo instantáneo en Watts (`W`)[cite: 5].
* **Cálculo de Ahorro Acumulado y Proyección Mensual:** Comparación directa en Watt-hora (`Wh`) entre la lámpara inteligente y la tradicional, además de la proyección de ahorro mensual en `kWh`[cite: 5].
* **Gráfico de Comportamiento Diario (`EnergyChart`):** Curvas comparativas de 24 horas que contrastan la luz natural disponible, la respuesta adaptativa de la lámpara inteligente y el consumo constante de una lámpara tradicional[cite: 5].

---

## 🧮 Modelo Matemático

El simulador implementa las siguientes ecuaciones para calcular el comportamiento del sensor LDR y el microcontrolador ESP32[cite: 5]:

| Etapa | Fórmula | Descripción |
| :--- | :--- | :--- |
| **01. Luz Solar** | $S(h) = \sin\left(\frac{\pi (h - 6)}{12}\right)$ para $6 < h < 18$ | Modela la curva de radiación solar diaria ($0$ fuera del rango diurno)[cite: 5]. |
| **02. Luz Ambiente** | $L_{amb} = S(h) \cdot \left(1 - \frac{0.8 \cdot N}{100}\right) \cdot 100$ | Ajusta la luz natural disponible según el porcentaje de nubosidad $N$[cite: 5]. |
| **03. Señal PWM** | $\text{PWM} = \max\left(1, \text{round}\left(255 \cdot \left(1 - \frac{L_{amb}}{100}\right)\right)\right)$ | Invierte la lectura del sensor para regular el ciclo de trabajo del LED[cite: 5]. |
| **04. Potencia Instantánea** | $P = P_{\max} \cdot \frac{\text{PWM}}{255}$ | Calcula el consumo eléctrico en Watts en un instante dado[cite: 5]. |
| **05. Energía y Ahorro** | $E = \sum P \cdot \Delta t \quad \mid \quad \text{Ahorro} = \frac{E_{\text{trad}} - E_{\text{int}}}{E_{\text{trad}}} \cdot 100$ | Integra el consumo acumulado ($\Delta t = 0.02\text{ h}$) y determina el porcentaje de ahorro[cite: 5]. |

---

## 🛠️ Stack Tecnológico

* **Framework:** React 19 + TypeScript 5.7
* **Build Tool:** Vite 8
* **Estilos:** Tailwind CSS v4 + CSS Custom Properties (`Manrope` & `DM Mono`)[cite: 2, 4]
* **Formateo de Código:** Oxfmt
* **Gráficos:** SVG nativo renderizado reactivamente (sin librerías externas pesadas)[cite: 5]

---

## 📁 Estructura del Proyecto

```text
├── .figma/              # Metadatos de integración con Figma Make
├── public/              # Archivos estáticos públicos
├── src/
│   ├── App.tsx          # Lógica de simulación, componentes UI, escena SVG y gráficos
│   ├── index.css        # Variables de diseño, estilos responsivos y Tailwind CSS
│   ├── main.tsx         # Punto de entrada de la aplicación React
│   └── vite-env.d.ts    # Definiciones de tipos para Vite
├── index.html           # Plantilla HTML principal
├── package.json         # Dependencias y scripts del proyecto
├── tsconfig.json        # Configuración de TypeScript
└── vite.config.ts       # Configuración de Vite
