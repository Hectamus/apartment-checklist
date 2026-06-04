# Apartment Checklist

A collaborative move-in checklist for planning and tracking apartment essentials. Built with vanilla JavaScript and Firebase Realtime Database for live, synchronized edits between two users.

## Features

- **Live Sync**: Real-time updates between two devices using Firebase—both users see changes instantly
- **Comprehensive Checklist**: Pre-built list of 130+ items across 8 categories (bedroom, kitchen, bathroom, etc.)
- **Budget Tracking**: View total costs, amount spent, and remaining budget
- **Custom Items & Categories**: Add your own items and create new categories as needed
- **Persistent Storage**: Data saved locally and synced to the cloud; works offline and syncs when reconnected
- **Filtering & Sorting**: Filter by category or priority; sort by default, priority, or to-do first
- **Item Details**: Track where to buy, cost, notes, links, and assign items to users
- **Progress Tracking**: Visual progress bar showing completion percentage

## How to Use

### Basic Operations
- **Check off items**: Click the checkbox when you've purchased an item
- **Edit items**: Click the pencil icon to modify name, priority, cost, location, notes, links, or assignee
- **Add items**: Use the "+ Add item" button to add custom items to any category
- **Hide/Delete items**: Use the eye icon to hide built-in items or delete custom items
- **Add categories**: Create new categories (e.g., "🏠 Garage") with the "+ Add a category" button

### Filtering
- Filter by **category** (top row of buttons)
- Filter by **priority** (Day 1, Week 1, Anytime)
- **Sort** by default order, priority, or to-do items first

### Keyboard Shortcuts
- **Enter**: Submit a new item or category while typing
- **Escape**: Cancel adding/editing

## Technical Details

### Architecture
- **Frontend**: Single-file HTML/CSS/JavaScript vanilla implementation (~1000 lines)
- **Storage**: Dual persistence
  - **Local**: `localStorage` for instant loading and offline access
  - **Cloud**: Firebase Realtime Database for real-time sync
- **Sync**: Bidirectional sync with conflict detection (ignores echo writes)
- **Status**: Visual sync indicator shows connection status (synced / offline / connecting)

### Data Structure
```
{
  checked: { itemId: true/false },
  edits: { itemId: { name, pri, where, cost, note, link, assignee, hidden } },
  custom: [ { id, cat, name, pri, where, cost } ],
  customCats: [ "Category Name" ],
  catNames: { "Original Cat": "Renamed Cat" }
}
```

## Firebase Setup

This project uses **Firebase Realtime Database** for real-time synchronization between two users.

### Configuration
The Firebase credentials are embedded in the HTML (in the `FB_CONFIG` object). This is a standard practice for web-only Firebase apps—the API key is intentionally public.

### Security Considerations
By default, your database has **public read/write access**. To restrict access:

1. **Add Firebase Rules** (Recommended for privacy)
   - Go to Firebase Console → Your Project → Realtime Database → Rules
   - Restrict access to only your users:
   ```json
   {
     "rules": {
       "checklist": {
         ".read": "auth.uid != null",
         ".write": "auth.uid != null"
       }
     }
   }
   ```
   - You'll then need to add authentication (email/password or Google Sign-In) to the app

2. **For a Private Couple's Project (Current Setup)**
   - If you only share this URL with your girlfriend, the current public setup is fine
   - Just be aware anyone with the URL can access/modify your checklist
   - No changes needed unless you want to add authentication

## Built With

- **Vanilla JavaScript** (no dependencies)
- **Firebase Realtime Database** (real-time sync)
- **localStorage** (offline persistence)
- Built with [Claude](https://claude.ai)

## License

MIT

## Notes

- All data is stored in Firebase under `https://apartment-checklist-9427b-default-rtdb.firebaseio.com/checklist`
- localStorage keeps a cached copy for instant page loads
- Sync status indicator shows real-time connection state
