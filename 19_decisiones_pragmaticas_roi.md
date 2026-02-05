# Post 19: Pragmatismo: Por qué elegir un Monolito Distribuido es una decisión financiera 💰

Si hablas con un reclutador o un CTO, no solo quieren oír que sabes usar la tecnología más compleja. Quieren saber que sabes **gastar inteligentemente su dinero**.

En `finzapp_api`, elegí un **Monolito Distribuido con Vertical Slice** en lugar de microservicios puros. ¿Por qué? Por el **ROI (Retorno de Inversión)**.

### 💸 El Costo Oculto de los Microservicios
Muchos "picadores de código" saltan a los microservicios por moda. Un Ingeniero Senior ve los costos:
- Mayor complejidad de red.
- Necesidad de orquestación (Kubernetes).
- Latencia distribuida.
- Dificultad para mantener la consistencia de datos.

Para la etapa actual del negocio, esa inversión no se justifica.

### ✨ El Enfoque Senior: Escalabilidad Lógica
Al usar Vertical Slice, tenemos lo mejor de ambos mundos:
- **Simplicidad de Despliegue**: Un solo pipeline, un solo servidor (ahorro de costos operativos).
- **Aislamiento por Diseño**: Si mañana un módulo necesita escalar de forma independiente, extraerlo a un microservicio es trivial. Ya está aislado lógicamente.

### 🚀 De Creador a Estratega
Ser Senior significa entender que el **tiempo de desarrollo** y la **complejidad operativa** son deudas que el negocio debe pagar. Elegir la arquitectura más sencilla que resuelva el problema actual (sin cerrar puertas al futuro) es la marca de un verdadero constructor de productos.

¿Tus decisiones técnicas están basadas en el valor para el negocio o en lo que se ve bien en tu CV? 👇

#SoftwareArchitecture #Pragmatism #ROI #Startups #Backend #VerticalSlice #Microservices #SeniorEngineer
