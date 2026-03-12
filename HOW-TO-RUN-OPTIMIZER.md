<!--
  Reference: How to Run the Dlivery Project (Optimizer + Backend + Frontend)
  Source: c:\dlivery\hpwToRunOptimizer.md
-->

# How to Run the Dlivery Project

This project has three parts: **Optimizer** (Java/Spring Boot), **Backend** (NestJS), and **Frontend** (Next.js).

---

## 1. If You Don't Have Java or Maven

### You need Java (JDK 21). You do **not** need Maven installed.

The optimizer uses the **Maven Wrapper** (`mvnw` / `mvnw.cmd`). The wrapper will download Maven for you when you run it. You only need **Java 21** on your machine.

### Install Java 21 (Windows)

1. **Option A — Microsoft Build of OpenJDK (recommended)**  
   - Download: https://learn.microsoft.com/en-us/java/openjdk/download#openjdk-21  
   - Run the installer and note the install path (e.g. `C:\Program Files\Microsoft\jdk-21.0.x.x-hotspot`).

2. **Option B — Eclipse Temurin**  
   - Download: https://adoptium.net/temurin/releases/?version=21  
   - Choose Windows x64, JDK, and run the installer.

3. **Set `JAVA_HOME`** (if the installer didn't):  
   - Windows: System Properties → Environment Variables → New:  
     - Variable: `JAVA_HOME`  
     - Value: e.g. `C:\Program Files\Microsoft\jdk-21.0.10.7-hotspot`  
   - Add to `PATH`: `%JAVA_HOME%\bin`

4. **Check** in a new terminal:
   ```powershell
   java -version
   ```
   You should see something like `openjdk version "21.x.x"`.

### Maven

- **Do not install Maven** unless you want it for other projects.  
- Use the wrapper from the repo: in the `optimizer` folder run `.\mvnw.cmd` (Windows) or `./mvnw` (Linux/macOS). It will download Maven on first run.

---

## 2. Run the Full Project (Optimizer + Backend + Frontend)

### Prerequisites

- **Java 21** (see above).  
- **Node.js** (v18+). Install from https://nodejs.org if needed.  
- **npm** (comes with Node).

### One-time setup (from repo root)

```powershell
# Backend
cd backend
npm install
cd ..

# Frontend
cd frontend
npm install
cd ..
```

No extra install step for the optimizer (Maven wrapper handles it).

---

### Option A — Three separate windows (PowerShell)

From the repo root (`c:\dlivery`):

```powershell
.\run-all.ps1
```

This opens three windows:

| Window   | Service   | Port | URL                    |
|----------|-----------|------|------------------------|
| 1        | Optimizer | 8081 | http://localhost:8081  |
| 2        | Backend   | 3004 | http://localhost:3004   |
| 3        | Frontend  | 3000 | http://localhost:3000  |

Use the app at **http://localhost:3000**.

If your JDK is not at `C:\Program Files\Microsoft\jdk-21.0.10.7-hotspot`, edit `run-all.ps1` and set `$jdkPath` to your `JAVA_HOME` path.

---

### Option B — Run everything in one terminal (concurrent)

Open one terminal at repo root and run each in the background, or use two other terminals:

**Terminal 1 — Optimizer**

```powershell
$env:JAVA_HOME = "C:\Program Files\Microsoft\jdk-21.0.10.7-hotspot"
$env:PATH = "$env:JAVA_HOME\bin;" + $env:PATH
cd c:\dlivery\optimizer
.\mvnw.cmd spring-boot:run
```

**Terminal 2 — Backend**

```powershell
cd c:\dlivery\backend
npm run start:dev
```

**Terminal 3 — Frontend**

```powershell
cd c:\dlivery\frontend
npm run dev
```

Then open **http://localhost:3000**.

---

### Option C — Optimizer only (e.g. to test the solver)

From repo root:

```powershell
$env:JAVA_HOME = "C:\Program Files\Microsoft\jdk-21.0.10.7-hotspot"
$env:PATH = "$env:JAVA_HOME\bin;" + $env:PATH
cd c:\dlivery\optimizer
.\mvnw.cmd spring-boot:run
```

Or from any directory, if `JAVA_HOME` is already set:

```powershell
cd c:\dlivery\optimizer
.\mvnw.cmd spring-boot:run
```

Optimizer API: **http://localhost:8081**.

---

## 3. Ports summary

| Service   | Port |
|----------|------|
| Frontend | 3000 |
| Backend  | 3004 |
| Optimizer| 8081 |

---

## 4. Troubleshooting

- **"JAVA_HOME not set" or "java not found"**  
  Install JDK 21 and set `JAVA_HOME` and `PATH` as in section 1. Restart the terminal after changing environment variables.

- **"mvn not found"**  
  You don't need `mvn` on the PATH. Use the wrapper: `cd optimizer` then `.\mvnw.cmd spring-boot:run` (Windows) or `./mvnw spring-boot:run` (Linux/macOS).

- **`run-all.ps1` can't find Java**  
  Edit `run-all.ps1` and set `$jdkPath` to your actual JDK path (same as `JAVA_HOME`).

- **Backend or frontend won't start**  
  Make sure you ran `npm install` in `backend` and `frontend` at least once.
