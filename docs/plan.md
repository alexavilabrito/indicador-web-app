

Implementar esta funcionalidad utilizando:

- Frontend: React 19, TypeScript y Vite
- Componentes UI: Material UI
- Gráficos: Apache ECharts
- Backend/BFF: NestJS con TypeScript
- Integración externa: API REST de mindicador.cl
- Caché: Redis
- Persistencia: PostgreSQL cuando sea necesaria
- Pruebas frontend: Vitest y React Testing Library
- Pruebas backend: Jest
- Pruebas E2E: Playwright
- Calidad: ESLint, Prettier y SonarQube
- Seguridad: Trivy y controles OWASP
- Contenedores: Docker
- CI/CD: Jenkins

Arquitectura:

- Frontend desacoplado del proveedor externo.
- Backend BFF como único punto de acceso a mindicador.cl.
- Adaptador anticorrupción para transformar las respuestas externas a un modelo interno.
- TypeScript en modo estricto.
- Diseño mobile-first y cumplimiento WCAG 2.2 AA.
- Cálculos financieros con aritmética decimal.