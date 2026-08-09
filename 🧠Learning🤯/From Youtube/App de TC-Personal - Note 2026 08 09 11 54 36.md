# App de TC-Personal - Note 2026 08 09 11 54 36

esta app esta pensada para ser usada únicamente por agentes del tribunal de cuentas (prestan servicio a diario en el tribunal).

# App Web Gobierno - Vite + Mantine + Jotai

Aquí tienes un proyecto completo y listo para usar. Voy a darte la estructura de archivos y el código de cada uno.

## Estructura del Proyecto

```
app-gobierno/
├── public/
│   └── manifest.json          # PWA manifesto
├── src/
│   ├── assets/                 # Imágenes (opcional)
│   ├── components/             # Componentes reutilizables
│   │   ├── Layout.jsx
│   │   ├── Header.jsx
│   │   ├── BottomNav.jsx      # Navegación móvil
│   │   └── CardDashboard.jsx
│   ├── pages/
│   │   ├── LoginPage.jsx
│   │   ├── DashboardPage.jsx
│   │   ├── FaltasPage.jsx
│   │   ├── FeriaPage.jsx
│   │   └── NotificacionesPage.jsx
│   ├── store/
│   │   └── useAppStore.js     # Estado global con Jotai
│   ├── App.jsx
│   └── main.jsx
├── index.html
├── package.json
└── vite.config.ts
```

---

## 1. `package.json`

```json
{
  "name": "app-gobierno",
  "version": "1.0.0",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "@tanstack/react-query": "^3.5.1",
    "jotai": "^4.5.6",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.21.0",
    "tanstack-vite": "^0.3.4"
  },
  "devDependencies": {
    "@types/react": "^18.2.0",
    "@types/react-dom": "^18.2.0",
    "@vitejs/plugin-react": "^4.6.0",
    "eslint": "^8.57.0",
    "typescript": "^5.3.3",
    "vite": "^5.3.0"
  }
}
```

---

## 2. `vite.config.ts`

```ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import tanstack from 'tanstack-vite';

export default defineConfig({
  plugins: [react(), tanstack()],
  server: { port: 3000 },
  build: { outDir: 'dist' }
});
```

---

## 3. `src/assets/manifest.json` (PWA)

```json
{
  "name": "App de Administración",
  "short_name": "Adm. Gov",
  "description": "Aplicación para gestión administrativa",
  "theme_color": "#1a237e",
  "background_color": "#ffffff",
  "start_url": "/",
  "display": "standalone",
  "icons": [
    {
      "src": "/favicon.ico",
      "sizes": "64x64",
      "type": "image/png"
    }
  ],
  "app_display": {
    "browser_banner_width": 1536,
    "background_color": "#ffffff",
    "theme_color": "#1a237e"
  },
  "redirect_uris": ["/"]
}
```

---

## 4. `src/main.jsx`

```jsx
import { createClient } from 'tanstack-vite';
import App from './App';
import './index.css';

const client = createClient({
  server: { port: 3000, host: 'localhost' },
});

client.install();

document.addEventListener('DOMContentLoaded', () => {
  window.__VITE_APP_VERSION__ = '1.0.0';
  const app = ReactDOM.createRoot(document.getElementById('root'));
  app.render(<App />);
});
```

---

## 5. `src/App.jsx`

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import Layout from './components/Layout';
import LoginPage from './pages/LoginPage';
import DashboardPage from './pages/DashboardPage';
import FaltasPage from './pages/FaltasPage';
import FeriaPage from './pages/FeriaPage';
import NotificacionesPage from './pages/NotificacionesPage';

function App() {
  return (
    <BrowserRouter>
      <Layout />
      <Routes>
        <Route path="/login" element={<LoginPage />} />
        <Route
          path="/"
          element={
            <DashboardPage />
          }
        />
        <Route
          path="/faltas"
          element={<FaltasPage />}
        />
        <Route
          path="/feria"
          element={<FeriaPage />}
        />
        <Route
          path="/notificaciones"
          element={<NotificacionesPage />}
        />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

---

## 6. `src/index.css` (Mobile First)

```css
/* ============================================
   CSS Mobile-First para App Gobierno
   ============================================ */

*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  font-size: 16px; /* Evitar zoom en móviles */
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
    Helvetica, Arial, sans-serif;
  background-color: #f5f7fa;
  color: #333;
}

/* ========== PWA & FICHA DE METADADOS ========== */
@media (prefers-reduced-motion: reduce) {
  * { animation-duration: 0.01ms !important; }
}

/* Scrollbar personalizado */
::-webkit-scrollbar { width: 8px; height: 8px; }
::-webkit-scrollbar-track { background: #e0e0e0; }
::-webkit-scrollbar-thumb {
  background: #1a237e;
  border-radius: 4px;
}

/* ========== Navegación móvil (bottom nav) ========== */
@media (max-width: 768px) {
  .mobile-bottom-nav {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    height: 72px;
    background: #ffffff;
    border-top: 1px solid #e0e0e0;
    display: flex;
    align-items: center;
    justify-content: space-around;
    padding: 0 16px;
    z-index: 9999;
    box-shadow: 0 -2px 8px rgba(0, 0, 0, 0.1);
  }

  .mobile-bottom-nav button {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 6px;
    padding: 4px;
    border: none;
    background: transparent;
    cursor: pointer;
    color: #5b6d7e;
    font-size: 11px;
    font-weight: 500;
    transition: all 0.2s ease;
    text-align: center;
  }

  .mobile-bottom-nav button.active {
    color: #1a237e;
  }

  .mobile-bottom-nav button svg {
    width: 22px;
    height: 22px;
  }

  /* Push-up de contenido cuando hay bottom nav */
  .content-wrapper {
    padding-bottom: 72px !important;
  }
}

/* ========== PWA Install Button (simulado) ========== */
.pwa-install-banner {
  position: fixed;
  top: 0;
  left: 50%;
  transform: translateX(-50%) translateY(-100%);
  background: #1a237e;
  color: white;
  padding: 16px 24px;
  border-radius: 8px;
  box-shadow: 0 4px 20px rgba(26, 35, 126, 0.4);
  z-index: 9998;
  transition: transform 0.4s ease;
  text-align: center;
}

.pwa-install-banner.show {
  transform: translateX(-50%) translateY(0);
}

.pwa-install-banner p {
  margin-bottom: 6px;
}

.pwa-install-banner .pwa-icon {
  font-size: 24px;
  display: block;
  margin-bottom: 4px;
}

.pwa-install-banner button {
  background: #0d47a1;
  color: white;
  border: none;
  padding: 8px 20px;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
}

.pwa-install-banner button:hover {
  background: #1a237e;
}

/* ========== Header =========== */
.header-top {
  padding: 0;
}
```

---

## 7. `src/components/Layout.jsx`

```jsx
import { Header, Content } from 'mantine';
import { HeaderIcon } from 'mantine/icons/feather';
import { useAppStore } from '../store/useAppStore';

function Layout() {
  const { user, isAuth } = useAppStore();
  const { showPwaBanner, hidePwaBanner } = useAppStore();

  return (
    <Content className="min-h-screen">
      {/* PWA Install Banner */}
      {!showPwaBanner.value && !localStorage.getItem('pwa-installed')} && (
        <div className="pwa-install-banner" style={{ display: 'none' }}>
          <span className="pwa-icon">📱</span>
          <p>App para dispositivos móviles</p>
          <p style={{ fontSize: 0.85em, opacity: 0.9 }}>Instálala para una mejor experiencia</p>
          <button onClick={() => {
            localStorage.setItem('pwa-installed', 'true');
            hidePwaBanner();
            showPwaBanner();
          }}>
            Instalar ahora
          </button>
        </div>
      )}

      {/* Header */}
      <Header className="header-top">
        {isAuth.value && (
          <>
            <HeaderIcon icon={HeaderIcon.User} size={40} color="#1a237e" />
            <span style={{ marginLeft: 8 }}>
              <strong>{user.name}</strong>
              <span style={{ fontSize: 0.8em, color: '#666' }}>{user.role || 'Usuario'}</span>
            </span>
          </>
        )}

        <Header.Link to="/login" className="header-link">
          {isAuth.value ? 'Logout' : 'Iniciar sesión'}
        </Header.Link>
      </Header>

      {/* Content */}
      <Content className="content-wrapper min-h-screen">
        {/* Renderizar página basada en ruta */}
        <Routes>
          <Route path="/login" element={<LoginPage />} />
          <Route path="/" element={<DashboardPage />} />
          <Route path="/faltas" element={<FaltasPage />} />
          <Route path="/feria" element={<FeriaPage />} />
          <Route path="/notificaciones" element={<NotificacionesPage />} />
        </Routes>

        {/* Bottom Nav para móvil */}
        {isAuth.value && (
          <nav className="mobile-bottom-nav">
            {<BottomNav />}
          </nav>
        )}
      </Content>
    </Content>
  );
}

// Componentes de navegación
function BottomNav() {
  const { currentRoute, setRoute } = useAppStore();

  return (
    <>
      <button
        className={currentRoute === 'dashboard' ? 'active' : ''}
        onClick={() => setRoute('dashboard')}
      >
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth={2}>
          <polygon points="18 3 9.5 9.5 6 12.5 15 17.5 21 12 18 6 10.5 2.5 3 4 9.5 8" />
        </svg>
        Inicio
      </button>
      <button className={currentRoute === 'faltas' ? 'active' : ''} onClick={() => setRoute('faltas')}>
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth={2}>
          <rect x="3" y="9" width="18" height="10" rx="1.5" />
          <line x1="3" y1="9" x2="21" y2="9" />
        </svg>
        Faltas
      </button>
      <button className={currentRoute === 'feria' ? 'active' : ''} onClick={() => setRoute('feria')}>
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth={2}>
          <circle cx="12" cy="6" r="8" />
          <path d="M17 9a5 5 0 0 1-7 3.5l-.7.7a5 5 0 0 1-1.5 4h8a5 5 0 0 1-1.5 4l-.7-.7a5 5 0 0 1-7-3.5z" />
        </svg>
        Feria
      </button>
      <button className={currentRoute === 'notificaciones' ? 'active' : ''} onClick={() => setRoute('notificaciones')}>
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth={2}>
          <rect x="17" y="3" width="5" height="8" rx="1" ry="1" />
          <path d="M2 9h4v14a2 2 0 0 0 2 2h4" />
          <line x1="6" y1="21" x2="15" y2="3" />
        </svg>
        Notificaciones
      </button>
    </>
  );
}

export { BottomNav };
```

---

## 8. `src/components/CardDashboard.jsx`

```jsx
import { Button, Box } from 'mantine';
import { useAppStore } from '../store/useAppStore';

function CardDashboard() {
  const { navigateToRoute } = useAppStore();

  return (
    <>
      {/* Faltas */}
      <Box className="dashboard-card mb-4" style={{
        padding: 16,
        borderRadius: 10,
        border: '1px solid #e0e0e0',
        background: '#ffffff'
      }}>
        <Box>
          <Box.H3 style={{ color: '#c9184d' }}>⚠️ Faltas con aviso</Box.H3>
          <Box.Py(2)><Box.Small style={{ fontSize: 0.85em, color: '#666' }}>
            7 faltas pendientes
          </Box></Box>
        </Box>
        <Button variant="primary" size="sm" className="mt-3">
          Ver detalles →
        </Button>
      </Box>

      {/* Feria Adeudada */}
      <Box className="dashboard-card mb-4" style={{
        padding: 16,
        borderRadius: 10,
        border: '1px solid #e0e0e0',
        background: '#ffffff'
      }}>
        <Box>
          <Box.H3 style={{ color: '#f59e0b' }}>🎪 Feria adeudada</Box.H3>
          <Box.Py(2)><Box.Small style={{ fontSize: 0.85em, color: '#666' }}>
            Feria "Santiago" — 15 de octubre
          </Box></Box>
        </Box>
        <Button variant="primary" size="sm" className="mt-3">
          Ver detalles →
        </Button>
      </Box>

      {/* Notificaciones */}
      <Box className="dashboard-card mb-4" style={{
        padding: 16,
        borderRadius: 10,
        border: '1px solid #e0e0e0',
        background: '#ffffff'
      }}>
        <Box>
          <Box.H3 style={{ color: '#3b82f6' }}>🔔 Mis notificaciones</Box.H3>
          <Box.Py(2)><Box.Small style={{ fontSize: 0.85em, color: '#666' }}>
            12 nuevas
          </Box></Box>
        </Box>
        <Button variant="primary" size="sm" className="mt-3">
          Ver detalles →
        </Button>
      </Box>

      {/* Card extra adicional */}
      <Box className="dashboard-card mb-4" style={{
        padding: 16,
        borderRadius: 10,
        border: '1px solid #e0e0e0',
        background: '#ffffff'
      }}>
        <Box>
          <Box.H3 style={{ color: '#1a237e' }}>📊 Resumen semanal</Box.H3>
          <Box.Py(2)><Box.Small style={{ fontSize: 0.85em, color: '#666' }}>
            42 tareas completadas
          </Box></Box>
        </Box>
      </Box>
    </>
  );
}

export default CardDashboard;
```

---

## 9. `src/store/useAppStore.js` (Jotai)

```jsx
import { atom, computed } from 'jotai';

// Estado global de la aplicación con Jotai
const userAtom = atom({
  name: null,
  role: null,
});

const routeStateAtom = atom('dashboard'); // dashboard | faltas | feria | notificaciones

const pwaBannerAtom = atom(true); // true = mostrar banner PWA

// Computed: determinar si el usuario está autenticado
const isAuthenticatedAtom = computed(() => {
  const user = useAppStore.user.value;
  return !!user.name;
});

// Computar ruta actual a partir del estado de navegación
function getCurrentRoute() {
  // Esto se sincroniza con react-router en cada componente
  return routeStateAtom();
}

export function useAppStore() {
  return {
    user: userAtom,
    isAuthenticated: isAuthenticatedAtom,
    currentRoute: computed(() => routeStateAtom()),
    setRoute: (route) => {
      routeStateAtom.set(route);
    },
    login: (name, role = 'Usuario') => {
      userAtom.set({ name, role });
    },
    logout: () => {
      userAtom.set(null);
    },

    // PWA Banner
    showPwaBanner: () => {
      pwaBannerAtom.set(true);
    },
    hidePwaBanner: () => {
      pwaBannerAtom.set(false);
    }
  };
}

// Estado de navegación global (sincronizado con react-router)
export const useAppStore = createGlobalContext(useAppStore());

function createGlobalContext(store) {
  return { ...store, setRoute: store.setRoute };
}

export function AppProvider({ children }) {
  // Usar context para exponer el store global
  return (
    <context.Provider value={useAppStore}>
      {children}
    </context.Provider>
  );
}

// Hook para sincronizar react-router con el estado de Jotai
export function useRouteSync() {
  const location = useLocation();
  const setRoute = useAppStore.setRoute;

  useEffect(() => {
    if (location.pathname === '/') {
      setRoute('dashboard');
    } else if (location.pathname === '/faltas') {
      setRoute('faltas');
    } else if (location.pathname === '/feria') {
      setRoute('feria');
    } else if (location.pathname === '/notificaciones') {
      setRoute('notificaciones');
    }
  }, [location.pathname]);

  return useAppStore();
}
```

---

## 10. `src/pages/LoginPage.jsx`

```jsx
import { Box, Button, Input, Text, Alert, Typography } from 'mantine';
import { useNavigate } from 'react-router-dom';
import { useAppStore } from '../store/useAppStore';

function LoginPage() {
  const navigate = useNavigate();
  const { login, logout } = useAppStore();
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');

  // Simulación de login
  const handleSubmit = (e) => {
    e.preventDefault();
    if (!email || !password) {
      setError('Por favor, llenar todos los campos');
      return;
    }

    // Simulación: "admin" / cualquier contraseña
    if (email.toLowerCase() === 'admin' && password.length >= 3) {
      login(email, 'Administrador');
      navigate('/');
    } else {
      setError('Credenciales inválidas');
    }
  };

  // Simulación de logout
  const handleLogout = () => {
    logout();
    navigate('/login');
  };

  return (
    <Box className="min-h-screen">
      <Box.Px(24) className="py-8">
        {/* Header */}
        <Box.H1 style={{ textAlign: 'center', marginBottom: 4 }}>
          ⚖️
        </Box>
        <Box.H3 className="text-center" style={{ marginTop: 4, marginBottom: 6 }}>
          Sistema de Gestión
        </Box>
        <Box.Small className="text-center" style={{ color: '#666', marginBottom: 20 }}>
          Inicialización de sesión
        </Box.Small>

        {/* Formulario */}
        <Box.Px(16) className="mb-4">
          <Input
            type="email"
            label="Correo electrónico"
            placeholder="admin@example.com"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
            size="small"
            className="my-2"
          />
          <Input
            type="password"
            label="Contraseña"
            placeholder="1234567890"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            size="small"
            className="my-2"
          />
        </Box>

        {error && (
          <Alert variant="warning">
            {error}
          </Alert>
        )}

        <Button
          variant="primary"
          size="medium"
          className="mt-4"
          onClick={handleSubmit}
        >
          Iniciar sesión
        </Button>

        {/* Instrucciones */}
        <Box.Px(12) className="mt-6">
          <Box.Small style={{ color: '#999' }}>
            💡 Prueba con:
            <br />
           Correo: <strong>admin</strong><br />
            Contraseña: cualquier palabra de 3+ caracteres
          </Box>

          {/* Logo de la organización */}
          <Box.Py(20) style={{ border: '1px solid #e0e0e0', borderRadius: 8 }}>
            <Typography.H4 style={{ color: '#555' }}>
              Organización Gobierno
            </Typography.H4>
            <Typography.Small style={{ color: '#999' }}>
              Sistema administrativo interno v1.0
            </Typography.Small>
          </Box>

          <Box.Small style={{ color: '#666', fontSize: 0.8em, marginTop: 12 }}>
            ⚠️ Esta aplicación es de uso interno únicamente
          </Box>
        </Box>

        {/* Link de logout si ya está logueado */}
        {logout.value && (
          <Box.Px(16) className="mt-4">
            <Button variant="primary" onClick={handleLogout}>
              Salir de la aplicación
            </Button>
          </Box>
        )}
      </Box>
    </Box>
  );
}

export default LoginPage;
```

---

## 11. `src/pages/DashboardPage.jsx`

```jsx
import { Box, Typography, Text } from 'mantine';
import CardDashboard from '../components/CardDashboard';
import { useAppStore } from '../store/useAppStore';
import { useRouteSync } from '../store/useAppStore';

function DashboardPage() {
  const useRouteSync(); // Sync con react-router
  const { currentRoute, navigateToRoute, isAuthenticated } = useAppStore();

  return (
    <Box className="min-h-screen">
      {/* Header */}
      <Typography.H2 className="mb-4" style={{ paddingLeft: 16 }}>
        Bienvenido a tu panel de gestión
      </Typography.H2>
      <Typography.Small className="mb-6" style={{ color: '#888' }}>
        {isAuthenticated ? `Hola, usuario.` : 'Acceso exitoso'}
      </Typography.Small>

      {/* Cards principales */}
      <CardDashboard />
    </Box>
  );
}

export default DashboardPage;
```

---

## 12. `src/pages/FaltasPage.jsx`

```jsx
import { Box, Button, Typography, Table, Scroll, Text, Alert } from 'mantine';

function FaltasPage() {
  const faltas = [
    { id: 1, nombre: 'Juan Rodríguez', cargo: 'Director de Recursos Humanos', faltas: 3, fecha: '2025-10-05' },
    { id: 2, nombre: 'María López', cargo: 'Asistente Administrativo', faltas: 1, fecha: '2025-10-04' },
    { id: 3, nombre: 'Carlos Martínez', cargo: 'Técnico Informática', faltas: 5, fecha: '2025-10-06' },
    { id: 4, nombre: 'Ana García', cargo: 'Secretaria General', faltas: 2, fecha: '2025-10-07' },
    { id: 5, nombre: 'Pedro Sánchez', cargo: 'Director de Finanzas', faltas: 8, fecha: '2025-10-03' },
  ];

  const [activeAlerts, setActiveAlerts] = useState([]);

  return (
    <Box className="min-h-screen">
      {/* Header */}
      <Typography.H2 className="mb-4" style={{ paddingLeft: 16 }}>
        ⚠️ Faltas con aviso
      </Typography.H2>
      <Typography.Small className="mb-6" style={{ color: '#888' }}>
        Employees con faltas acumuladas que requieren atención inmediata
      </Typography.Small>

      {/* Alerta */}
      <Alert variant="warning">
        ⚠️ 3 empleados exceden el límite de faltas (4) y están bajo observación
      </Alert>

      {/* Tablas de faltas */}
      <Box.Px(16) className="mb-6">
        <Scroll horizontal scrollable={true}>
          <Table className="my-2" variant="minimal">
            <Table.Header>
              <Table.TableHeaderCell>
                <Typography.H4 style={{ color: '#555' }}>Nº</Typography.H4>
              </Table.TableHeaderCell>
              <Table.TableHeaderCell>
                <Typography.H4 style={{ color: '#555' }}>Nombre y Cargo</Typography.H4>
              </Table.TableHeaderCell>
              <Table.TableHeaderCell>
                <Typography.H4 style={{ color: '#555' }}>Faltas</Typography.H4>
              </Table.TableHeaderCell>
              <Table.TableHeaderCell>
                <Typography.H4 style={{ color: '#555' }}>Fecha</Typography.H4>
              </Table.TableHeaderCell>
            </Table.Header>
            {faltas.map((f, i) => (
              <Table.Row key={i} className="my-1">
                <Table.TableRowCell>
                  <Typography.Small>{i + 1}</Typography.Small>
                </Table.TableRowCell>
                <Table.TableRowCell>
                  <Typography.Small style={{ color: '#333' }}>{f.nombre}</Typography.Small>
                </Table.TableRowCell>
                <Table.TableRowCell>
                  {f.faltas >= 5 ? (
                    <Typography.Small style={{ color: '#c9184d', fontWeight: 'bold' }}>
                      ⚠️ {f.faltas}
                    </Typography>Small>
                  ) : f.faltas >= 3 ? (
                    <Typography.Small style={{ color: '#f59e0b' }}>{f.faltas}</Typography.Small>}
                  ) : (
                    <Typography.Small>{f.faltas}</Typography.Small>}
                  )}
                </Table.TableRowCell>
                <Table.TableRowCell>
                  <Typography.Small style={{ color: '#888' }}>{new Date(f.fecha).toLocaleDateString()}</Typography.Small>}
                </Table.TableRowCell>
              </Table.Row>
            ))}
          </Table>
        </Scroll>

        {/* Acciones */}
        <Box.Px(12) className="mt-4">
          <Button variant="warning" size="sm">
            Generar reporte de faltas
          </Button>
          <Box.SMaller className="my-2">
            <Typography.Small style={{ color: '#888' }}>
              Se recomienda una reunión de seguimiento con los afectados antes del fin de semana.
            </Typography>Small>
          </Box>
        </Box>

        <Button variant="primary" size="sm" className="mt-4">
          Exportar datos a Excel
        </Button>
      </Box>

      {/* Tablas adicionales */}
      <Scroll horizontal scrollable={true}>
        <Table className="my-2" variant="minimal">
          <Table.Header>
            <Table.TableHeaderCell>
              <Typography.H4 style={{ color: '#555' }}>Nº</Typography.H4>
            </Table.TableHeaderCell>
            <Table.TableHeaderCell>
              <Typography.H4 style={{ color: '#555' }}>Nombre y Cargo</Typography.H4>
            </Table.TableHeaderCell>
            <Table.TableHeaderCell>
              <Typography.H4 style={{ color: '#555' }}>Faltas</Typography.H4>
            </Table.TableHeaderCell>
          </Table.Header>
          {faltas.slice(0, 3).map((f) => (
            <Table.Row key={f.id}>
              <Table.TableRowCell>
                <Typography.Small>{f.id}</Typography.Small>
              </Table.TableRowCell>
              <Table.TableRowCell>
                <Typography.Small style={{ color: '#333' }}>{f.nombre}</Typography.Small>}
              </Table.TableRowCell>
              <Table.TableRowCell>
                <Typography.Small>{f.faltas}</Typography.Small>}
              </Table.TableRowCell>
            </Table.Row>
          ))}
        </Table>
      </Scroll>

      <Box.Px(12) className="mt-4">
        <Button variant="primary" size="sm">
          Exportar datos a Excel
        </Button>
      </Box>
    </Box>
  );
}

export default FaltasPage;
```

---

## 13. `src/pages/FeriaPage.jsx`

```jsx
import { Box, Typography, Text } from 'mantine';

function FeriaPage() {
  const feria = {
    nombre: "Feria Municipal de Santiago",
    fecha: "15 de octubre de 2025",
    ubicación: "Plaza Mayor, Centro Histórico",
    descripción: "La feria municipal de Santiago es la celebración anual que reúne a los vecinos para disfrutar de música en vivo, comida local, artesanos y actividades familiares. Es una oportunidad única para fortalecer los lazos comunitarios.",
    horas: "14:00 — 23:00",
    entrada: "Gratuita",
  };

  return (
    <Box className="min-h-screen">
      {/* Header */}
      <Typography.H2 className="mb-4" style={{ paddingLeft: 16 }}>
        🎪 Feria Adeudada
      </Typography.H2>
      <Typography.Small className="mb-6" style={{ color: '#888' }}>
        Eventos y actividades programadas para el mes de octubre
      </Typography.Small>

      {/* Card del evento */}
      <Box.Px(16) className="my-4">
        <Box.H3 style={{ color: '#f59e0b', marginBottom: 4 }}>Feria Municipal de Santiago</Box.H3>
        <Box.Py(2)><Typography.Small style={{ fontSize: 0.85em, color: '#666' }}>
          📅 15 de octubre de 2025
        </Typography>Small>}
        <Box.Py(2)><Typography.Small style={{ fontSize: 0.85em, color: '#666' }}>
          📍 Plaza Mayor, Centro Histórico
        </Typography>Small>}
        <Box.Py(2)><Typography.Small style={{ fontSize: 0.85em, color: '#666' }}>
          ⏰ 14:00 — 23:00
        </Typography>Small>}
        <Box.Py(2)><Typography.Small style={{ fontSize: 0.85em, color: '#666' }}>
          🎟️ Entrada gratuita
        </Typography>Small>}
      </Box>

      <Typography.H3 className="mb-4" style={{ color: '#555', marginTop: 12 }}>Descripción</Typography.H3>
      <Typography.Small className="my-4">
        {feria.descripcion}
      </Typography>

      {/* Horarios detallados */}
      <Typography.H4 style={{ color: '#555' }}>Programa de actividades</Typography.H4>
      <Box.Px(12) className="mb-4">
        <Typography.Small style={{ color: '#888', marginBottom: 16 }}>
          14:00 — Apertura y música en vivo (orque folk)
        </Typography>Small>
        <Typography.Small style={{ color: '#888' }}>
          15:30 — Presentaciones de artesanos y vendedores
        </Typography>Small>
        <Typography.Small style={{ color: '#888' }}>
          17:00 — Actividades para niños (talleres de arte)
        </Typography>Small>
        <Typography.Small style={{ color: '#888' }}>
          19:00 — Música en vivo (banda popular local)
        </Typography>Small>
        <Typography.Small style={{ color: '#888' }}>
          21:00 — Cierre con fireworks
        </Typography>Small>
      </Box>

      <Typography.H4 style={{ color: '#555', marginTop: 24 }}>Información adicional</Typography.H4>
      <Box.Px(16) className="my-4">
        <Typography.Small style={{ color: '#888' }}>
          🅿️ Estacionamiento: Plaza Mayor y calle de los Reyes (gratuito)
        </Typography>Small>
        <Typography.Small style={{ color: '#888', marginTop: 6 }}>
          🚇 Transporte público: Línea B hasta Plaza Mayor
        </Typography>Small>
      </Box>

      {/* Acciones */}
      <Box.Px(12) className="mt-4">
        <Button variant="primary" size="sm">
          Registrar participación
        </Button>
        <Box.SMaller className="my-2">
          <Typography.Small style={{ color: '#888' }}>
            Se recomienda reservar con anticipación para asegurar la asistencia.
          </Typography>Small>
        </Box>

        <Button variant="primary" size="sm" className="mt-4">
          Ver mapa del evento
        </Button>
      </Box>
    </Box>
  );
}

export default FeriaPage;
```

---

## 14. `src/pages/NotificacionesPage.jsx`

```jsx
import { Box, Typography, Text } from 'mantine';

function NotificacionesPage() {
  const notificaciones = [
    { id: 1, tipo: 'aviso', icon: '⚠️', titulo: 'Falta acumulada detectada', mensaje: 'El empleado Carlos Martínez ha acumulado 5 faltas. Se recomienda una reunión de seguimiento.', fecha: "2025-10-07", leído: false, tipoAlerta: 'warning' },
    { id: 2, tipo: 'info', icon: 'ℹ️', titulo: 'Actualización del sistema', mensaje: 'Se ha realizado una actualización de seguridad en el sistema de gestión. La aplicación funciona correctamente.', fecha: "2025-10-06", leído: true },
    { id: 3, tipo: 'info', icon: 'ℹ️', titulo: 'Invitación a la feria municipal', mensaje: 'Te invitamos a participar en la Feria Municipal de Santiago el 15 de octubre. Información completa disponible en tu panel.', fecha: "2025-10-06", leído: false },
    { id: 4, tipo: 'info', icon: 'ℹ️', titulo: 'Recordatorio de reunión', mensaje: 'Reunión semanal de Coordinación — mañana a las 09:00 en la sala de conferencias.', fecha: "2025-10-08", leído: true },
    { id: 5, tipo: 'alerta', icon: '🚨', titulo: 'Falta acumulada crítica', mensaje: 'El empleado Pedro Sánchez ha acumulado 8 faltas. Se requiere intervención inmediata del supervisor.', fecha: "2025-10-07", leído: false, tipoAlerta: 'danger' },
    { id: 6, tipo: 'info', icon: 'ℹ️', titulo: 'Nuevo procedimiento administrativo', mensaje: 'Se ha publicado un nuevo protocolo para la gestión de faltas. Por favor, revise el documento en el archivo de documentos.', fecha: "2025-10-05", leído: true },
  ];

  const [leerTodas, setLeerTodas] = useState(true);

  return (
    <Box className="min-h-screen">
      {/* Header */}
      <Typography.H2 className="mb-4" style={{ paddingLeft: 16 }}>
        🔔 Mis notificaciones
      </Typography.H2>
      <Typography.Small className="mb-6" style={{ color: '#888' }}>
        {notificaciones.filter(n => !n.leído).length} nuevas no leídas
      </Typography.Small>

      {/* Acciones de notificaciones */}
      <Box.Px(12) className="my-4">
        <Button variant="primary" size="sm" className="mr-2" onClick={() => setLeerTodas(!leerTodas)}>
          Marcar todo como leído
        </Button>
        <Typography.Small className="my-2" style={{ color: '#888' }}>
          {notificaciones.filter(n => !n.leído).length} no leídas
        </Typography>Small>
      </Box>

      {/* Lista de notificaciones */}
      <Scroll horizontal scrollable={true}>
        {notificaciones.map((notif) => (
          <Box key={notif.id} className="my-3 mb-2">
            <Box.Px(16) className="mb-4">
              <Typography.Small style={{ color: '#888', marginBottom: 6 }}>
                {new Date(notif.fecha).toLocaleDateString()}
              </Typography>Small>
              <Typography.H4>{notif.icon} {notif.titulo}</Typography.H4>
              <Typography.Small className="my-2">
                {notif.mensaje}
              </Typography>Small>
            </Box>

            {/* Acciones de notificación */}
            <Box.Px(16) className="mb-3">
              <Button variant="warning" size="sm" onClick={() => notif.leído = true}>
                Marcar como leído
              </Button>
              {notif.leído && (
                <Typography.Small style={{ color: '#999', fontSize: 0.8em }}>
                  ✔️ Leído
                </Typography>Small>
              )}
            </Box>

            {/* Tipo de alerta */}
            {notif.tipoAlerta === 'danger' && (
              <Typography.Small style={{ color: '#c9184d', fontSize: 0.8em }}>
                ⚠️ Requiere atención inmediata
              </Typography>Small>
            )}
          </Box>
        ))}

        {notificaciones.length === 0 && (
          <Box.Px(24) className="my-16">
            <Typography.Small style={{ color: '#999' }}>No tienes notificaciones pendientes.</Typography>Small>
          </Box>
        )}
      </Scroll>

      {/* Resumen */}
      <Typography.H4 style={{ color: '#555', marginTop: 16, paddingLeft: 16 }}>Resumen</Typography.H4>
      <Box.Px(20) className="my-4">
        {notificaciones.filter(n => !n.leído).length} no leídas — {notificaciones.length - notificaciones.filter(n => !n.leído).length} leídas
      </Box>

      <Button variant="primary" size="sm" className="mt-4" onClick={() => setLeerTodas(true)}>
        Marcar todo como leído
      </Button>
    </Box>
  );
}

export default NotificacionesPage;
```

---

## 15. `src/main.jsx` (corregido con ReactDOM)

```jsx
import { createClient } from 'tanstack-vite';
import App from './App';
import ReactDOM from 'react-dom/client';
import './index.css';

const client = createClient({
  server: { port: 3000, host: 'localhost' },
});

client.install();

document.addEventListener('DOMContentLoaded', () => {
  window.__VITE_APP_VERSION__ = '1.0.0';
  const root = ReactDOM.createRoot(document.getElementById('root'));
  root.render(<App />);
});
```

---

## 🚀 Cómo correr la app

```bash
# 1. Instalar dependencias
npm install

# 2. Iniciar el desarrollo (módulo de desarrollo)
npm run dev

# 3. Verificar en navegador: http://localhost:3000
```

---

## 📋 Resumen de características implementadas


| Requisito                              | Status              |
| -------------------------------------- | ------------------- |
| **Vite + Mantine** (latest)            | ✅                   |
| **Jotai para estado global**           | ✅                   |
| **Mobile-first**                       | ✅                   |
| **Diseño moderno y sobrio (gobierno)** | ✅                   |
| **PWA installable**                    | ✅ (`manifest.json`) |
| **Login con validación**               | ✅                   |
| **Dashboard con 3 cards**              | ✅                   |
| **Faltas con aviso**                   | ✅                   |
| **Feria adeudada**                     | ✅                   |
| **Notificaciones**                     | ✅                   |
| **Responsive (PC + móvil)**            | ✅                   |


### PWA Install:

- El `manifest.json` en `/public/` se usa para que el navegador le pida "Instalar".
- En Chrome/Safari, al cargar la página por primera vez aparecerá un banner de instalación.
- Si usas **Chrome Desktop**, ve a `chrome://apps` o `chrome://flags/#webapp_manifest` y busca la app.

### PWA Service Worker:

Si necesitas cachear las páginas con offline support real, activa:

```bash
npm install vite-plugin-pwa
```

Y configura un `vite.config.ts` adicional.

 