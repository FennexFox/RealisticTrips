# Realistic Trips (Time2Work)

A comprehensive Cities: Skylines II mod that overhauls citizen behavior simulation with realistic daily schedules, work shifts, leisure activities, special events, and time mechanics.

## Features

- **Realistic Work Schedules** — Citizens follow configurable work hours with shift variations, part-time work, lunch breaks, overtime, and remote work support
- **Day-of-Week Simulation** — Full 7-day week cycle (or fixed day-type modes) with different behavior patterns for weekdays, Fridays, Saturdays, and Sundays
- **Leisure Activity Modeling** — Hourly probability profiles for meals, entertainment, shopping, parks, and travel based on time of day and day type
- **Special Events** — Attractions host scheduled events that draw citizens, with in-game chirp announcements (via CustomChirps integration)
- **Configurable Time Speed** — Adjustable time reduction factor for slower, more detailed simulation
- **Student Schedules** — School attendance with configurable start/end times and vacation months per education level
- **Tourism Overhaul** — Replacement tourist spawn and behavior systems
- **Economy Integration** — Custom economy and demand parameter systems that adapt to time-based activity
- **Multi-Language Support** — English (en-US) and Brazilian Portuguese (pt-BR) localizations
- **Modern UI** — React-based floating menu showing special events, and citizen schedule info panels

## Architecture

The mod replaces several vanilla game systems with custom ECS-based implementations:

```
NightShift/
├── Mod.cs                  # Entry point — disables vanilla systems, registers custom ones
├── Setting.cs              # Extensive mod configuration
├── Components/             # ECS data components (CitizenSchedule, Shopper, TruckSchedule, SpecialEventData)
├── Systems/                # 27 simulation & UI systems
│   ├── Time2WorkTimeSystem         # Core time management
│   ├── WeekSystem                  # Day-of-week tracking & probability updates
│   ├── Time2WorkCitizenBehaviorSystem  # Citizen sleep/wake/activity decisions
│   ├── Time2WorkWorkerSystem       # Work shifts, commute, lunch breaks
│   ├── Time2WorkStudentSystem      # School schedules
│   ├── Time2WorkLeisureSystem      # Leisure activity scheduling
│   ├── SpecialEventSystem          # Event creation & management
│   └── ...                         # Tourism, economy, UI systems
├── Patches/                # Harmony patches for vanilla TimeSystem
├── Utils/                  # Helpers (GaussianRandom, LeisureProbabilityCalculator, etc.)
├── Bridge/                 # External mod integration (CustomChirps)
├── Extensions/             # UI system base classes & binding helpers
├── Localization/           # Translation strings
└── Time2WorkUI/            # React/TypeScript UI components (webpack-bundled)
```

### Key Systems

| System | Replaces | Purpose |
|--------|----------|---------|
| `Time2WorkTimeSystem` | `TimeSystem` (patched) | Configurable ticks-per-day with time reduction factor |
| `WeekSystem` | — | Tracks day-of-week, updates off-day probabilities |
| `Time2WorkWorkerSystem` | `WorkerSystem` | Shift-aware work hours, commute timing, part-time/overtime |
| `Time2WorkLeisureSystem` | `LeisureSystem` | Time-based leisure probabilities by activity type |
| `Time2WorkStudentSystem` | `StudentSystem` | Per-level school schedules with vacation support |
| `SpecialEventSystem` | — | Generates events at attractions, biases pathfinding |

## Building

### Prerequisites

- **Visual Studio 2022** (or compatible MSBuild toolchain)
- **.NET Framework 4.8** targeting pack
- **Node.js ≥ 18** and **npm** (for UI build)
- **Cities: Skylines II** installed (for game assembly references)

### Build Steps

1. Clone the repository:
   ```bash
   git clone https://github.com/FennexFox/RealisticTrips.git
   ```

2. Update the game assembly path in `NightShift/Time2Work.csproj` if your game is not installed at the default Steam location:
   ```xml
   <GamePath>C:\Program Files (x86)\Steam\steamapps\common\Cities Skylines II\Cities2_Data\Managed</GamePath>
   ```

3. Install UI dependencies:
   ```bash
   cd NightShift/Time2WorkUI
   npm install
   ```

4. Build the solution:
   ```bash
   dotnet build Time2Work.sln
   ```
   This builds the C# DLL and automatically runs `npm run build` for the UI.

### Development

For UI hot-reload during development:
```bash
cd NightShift/Time2WorkUI
npm run dev
```

## Configuration

The mod exposes extensive settings through the in-game options menu, including:

- Work schedule parameters (start/end times, shift types, lunch breaks)
- Off-day probabilities per sector (office, commercial, industry, city services) and day type
- School schedules and vacation months
- Leisure activity multipliers
- Time speed factor
- Special event frequency
- Tourism parameters

## License

See repository for license information.
