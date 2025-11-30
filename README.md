# 🎓 Student Lecture Tracker

A responsive web application built with React and Tailwind CSS for managing student lectures and notes.

## 🌐 Live Demo

**[View Live App](https://hmgirlish0-cpu.github.io/student-lecture-tracker/)**

**Repository:** [GitHub](https://github.com/hmgirlish0-cpu/student-lecture-tracker)

## ✨ Features

- 📝 **Profile Management** - Save and edit student name and college information
- 📚 **Lecture Scheduling** - Add, edit, and delete daily lectures with subject and time
- 📓 **Note-Taking** - Dedicated text area for each lecture with Save and Clear buttons
- 💾 **Data Persistence** - All data automatically saved to browser's Local Storage
- 📱 **Fully Responsive** - Works seamlessly on mobile, tablet, and desktop devices
- 🎨 **Modern UI** - Clean design with Tailwind CSS and smooth animations

## 🚀 Quick Start

### Option 1: Open Directly
Simply open `index.html` in your browser - no installation required!

### Option 2: Run Local Server
```bash
# Using Node.js
node server.js

# Or using Python
python -m http.server 9000
```

Then open: http://localhost:9000

## 🛠️ Tech Stack

- **React 18** - UI framework (loaded from CDN)
- **Tailwind CSS** - Utility-first CSS framework (loaded from CDN)
- **Vanilla JavaScript** - No build tools required
- **Local Storage API** - Client-side data persistence

## 📱 Screenshots

### Desktop View
![Desktop View](https://via.placeholder.com/800x400?text=Desktop+View)

### Mobile View
![Mobile View](https://via.placeholder.com/400x600?text=Mobile+View)

## 💻 Usage

### Managing Profile
1. Enter your name and college name
2. Click "Save Profile" to persist your information

### Adding Lectures
1. Click the "+ Add New Lecture" button
2. Fill in subject and time
3. Click "Add" to create the lecture

### Taking Notes
1. Each lecture card has a notes section
2. Type your notes in the text area
3. Click "Save" to persist notes (shows "✓ Saved!" confirmation)
4. Click "Clear" to remove all notes

### Editing & Deleting
- Click "Edit" to modify lecture details
- Click "Delete" to remove a lecture (with confirmation)

## 🌍 Deployment

### GitHub Pages

1. Fork or clone this repository
2. Go to repository Settings → Pages
3. Select "main" branch and "/" (root) folder
4. Click Save
5. Your app will be live at: `https://yourusername.github.io/student-lecture-tracker/`

### Netlify

1. Go to [Netlify Drop](https://app.netlify.com/drop)
2. Drag and drop `index.html`
3. Get instant public URL

### Vercel

1. Go to [Vercel](https://vercel.com)
2. Import this repository
3. Deploy with one click

## 📂 Project Structure

```
student-lecture-tracker/
├── index.html          # Main application file (complete React app)
├── server.js           # Optional Node.js server
├── README.md           # This file
└── .gitignore         # Git ignore rules
```

## 🎯 Key Features Explained

### Responsive Design
- **Mobile** (< 640px): Single column layout
- **Tablet** (640px - 1024px): Two column grid
- **Desktop** (> 1024px): Three column grid

### Data Persistence
- All data automatically saved to Local Storage
- Persists across browser sessions
- Graceful error handling for corrupted data

### Validation
- Profile fields require non-empty values
- Lecture subject and time are required
- User-friendly error messages

## 🔧 Browser Support

Works on all modern browsers:
- ✅ Chrome 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Edge 90+
- ✅ Mobile browsers (iOS Safari, Chrome Mobile)

## 📝 License

MIT License - feel free to use this project for learning or personal use.

## 👨‍💻 Author

Built with ❤️ using React and Tailwind CSS

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

## ⭐ Show Your Support

Give a ⭐️ if you like this project!

---

**Note:** This is a client-side only application. All data is stored locally in your browser's Local Storage. No backend server or database is required.
