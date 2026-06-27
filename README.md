# Forex Pro Journal

A professional Forex trading journal mobile application built with React Native and Expo. Track your trades, analyze performance, and improve your trading strategy with detailed analytics and statistics.

## Features

### Core Features
- **Multi-Account Management**: Manage multiple trading accounts with different brokers
- **Trade Entry Form**: Quick and intuitive trade entry with real-time P&L calculations
- **Trading Calendar**: Visual calendar with profit/loss color coding (green for wins, red for losses)
- **Analytics Dashboard**: Comprehensive performance metrics including:
  - Win rate and profit factor
  - Average win/loss calculations
  - Risk-reward ratio analysis
  - Equity curve visualization
  - Monthly P&L charts
  - Win/loss distribution pie chart

### Advanced Features
- **Trade History**: Filter and sort trades by currency pair, date, or P&L
- **Detailed Trade View**: Complete trade information including entry/exit prices, lot size, and notes
- **Account Statistics**: Quick overview of account performance
- **Data Export**: Export trading data as JSON or CSV for backup and analysis
- **Dark/Light Mode**: Automatic theme switching based on system preferences

## Tech Stack

- **Framework**: React Native with Expo SDK 54
- **Language**: TypeScript
- **Styling**: NativeWind (Tailwind CSS for React Native)
- **State Management**: React Context + AsyncStorage
- **Charts**: React Native SVG with custom chart components
- **Build**: Expo with GitHub Actions for automated APK builds

## Getting Started

### Prerequisites
- Node.js 18+
- npm or pnpm
- Expo CLI (`npm install -g expo-cli`)

### Installation

```bash
# Clone the repository
git clone https://github.com/bitafam/ManusJournalApp.git
cd ManusJournalApp

# Install dependencies
npm install

# Start the development server
npm run dev
```

### Running on Mobile

#### iOS
```bash
npm run ios
```

#### Android
```bash
npm run android
```

#### Web
```bash
npm run web
```

## Project Structure

```
forex-journal-app/
├── app/                          # Expo Router screens
│   ├── (tabs)/                   # Tab-based navigation
│   │   ├── index.tsx             # Home screen
│   │   ├── calendar.tsx          # Trading calendar
│   │   ├── trade-entry.tsx       # Trade entry form
│   │   ├── analytics.tsx         # Analytics dashboard
│   │   └── settings.tsx          # Settings screen
│   ├── account-management.tsx    # Account management
│   ├── trade-history.tsx         # Trade history
│   └── trade-details/[id].tsx    # Trade details
├── components/                   # Reusable components
│   ├── screen-container.tsx      # SafeArea wrapper
│   └── chart-component.tsx       # Chart components
├── lib/                          # Utilities and hooks
│   ├── trade-context.tsx         # Global state
│   ├── storage.ts                # AsyncStorage utilities
│   ├── calculations.ts           # Trading calculations
│   └── types.ts                  # TypeScript types
├── assets/                       # Images and icons
└── theme.config.js               # Tailwind theme config
```

## Usage

### Creating an Account
1. Open the app and tap "Create Account"
2. Enter account name, initial balance, and currency
3. Start adding trades

### Adding a Trade
1. Tap the "Add Trade" button on the home screen
2. Fill in trade details (pair, entry price, exit price, lot size)
3. Optionally add stop loss and notes
4. P&L is calculated automatically
5. Tap "Save Trade"

### Viewing Analytics
1. Navigate to the Analytics tab
2. View key metrics: win rate, profit factor, average win/loss
3. Check equity curve, monthly P&L, and win/loss distribution
4. Analyze top performing currency pairs

### Exporting Data
1. Go to Settings
2. Choose "Export as JSON" or "Export as CSV"
3. Share or save the exported file

## GitHub Actions Workflow

This project includes an automated GitHub Actions workflow for building Android APK files.

### Workflow Features
- Automatic builds on push to `main` or `develop` branches
- Manual trigger via workflow dispatch
- Automated artifact uploads
- Release creation and APK upload on tags

### Triggering Builds

**Automatic (on push):**
```bash
git push origin main
```

**Manual (via GitHub UI):**
1. Go to Actions tab
2. Select "Build Android APK"
3. Click "Run workflow"

**Release (create tag):**
```bash
git tag v1.0.0
git push origin v1.0.0
```

## Building Locally

### Prerequisites
- EAS CLI: `npm install -g eas-cli`
- Expo account (free)

### Build Command
```bash
eas build --platform android --non-interactive
```

## Performance Metrics

The app calculates the following metrics for each trade:

| Metric | Description |
|--------|-------------|
| **P&L** | Profit or loss in currency units |
| **P&L %** | Profit or loss as percentage of entry price |
| **Win Rate** | Percentage of winning trades |
| **Profit Factor** | Gross profit ÷ Gross loss |
| **Average Win** | Average profit on winning trades |
| **Average Loss** | Average loss on losing trades |
| **Risk:Reward** | Risk per trade ÷ Reward per trade |
| **Largest Win** | Biggest single winning trade |
| **Largest Loss** | Biggest single losing trade |

## Data Storage

All data is stored locally on your device using AsyncStorage. No data is sent to external servers unless you explicitly export it.

## Future Enhancements

- Cloud sync with Firebase/Supabase
- Push notifications for trade reminders
- Biometric authentication
- Advanced charting library integration
- Trade replay functionality
- Sentiment analysis for trades
- Performance benchmarking

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is open source and available under the MIT License.

## Support

For issues, questions, or suggestions, please open an issue on GitHub.

## Changelog

### Version 1.0.0
- Initial release
- Multi-account support
- Trade entry and history
- Analytics dashboard with charts
- Data export functionality
- GitHub Actions CI/CD workflow

---

**Built with ❤️ for traders**
