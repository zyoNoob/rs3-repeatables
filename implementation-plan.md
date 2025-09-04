# RS3 Repeatables Enhancement Implementation Plan

## 🎯 Design Specifications

### **UI Layout Changes**
- **Header Layout**: Split into left and right sections
  - **Left**: Hamburger menu icon (☰)
  - **Right**: Profile icon with initial/name
- **Hamburger Menu**: Slide-out overlay panel from left containing:
  - Reset buttons (All, Monthly, Weekly, Daily)
  - Task density slider
  - Column count selector
  - Task hiding settings toggle
- **Profile Menu**: Dropdown from profile icon containing:
  - Current profile indicator
  - Profile selection dropdown
  - Create/Delete profile buttons

### **Task Hiding System Architecture**
```javascript
// Data Structure
globalSettings = {
  hiddenTasks: ["task-id-1", "task-id-2"],
  useGlobalHiding: true // user preference
}

profileSettings = {
  hiddenTasks: ["profile-specific-hidden"],
  density: 3,
  columns: 1,
  useGlobalHiding: false // can override global setting
}
```

### **Auto-Reset System Enhancement**
- **Global Reset Logic**: Apply resets to all profiles simultaneously
- **Cross-Profile Persistence**: Maintain reset timestamps globally
- **Notification System**: Show which profiles were affected by auto-reset

## 🔧 Technical Implementation

### **Phase 1: UI Components**
1. **Header Restructure**
   - Remove existing controls section
   - Add hamburger icon (top-left)
   - Add profile icon (top-right) with current profile initial

2. **Hamburger Menu Panel**
   - Slide-out animation from left
   - Semi-transparent backdrop
   - Contains all current control functionality
   - Mobile-responsive design

3. **Profile Dropdown Menu**
   - Positioned below profile icon
   - Profile management functions
   - Current profile highlighting

### **Phase 2: Enhanced Data Management**
1. **Profile Settings Storage**
   ```javascript
   profile = {
     name: "Profile Name",
     progress: { /* existing task states */ },
     settings: {
       density: 3,
       columns: 1,
       hiddenTasks: ["task-1", "task-2"],
       useGlobalHiding: false
     }
   }
   ```

2. **Global Settings Storage**
   ```javascript
   globalSettings = {
     hiddenTasks: ["global-hidden-1"],
     useGlobalHiding: true,
     lastResetCheck: { daily: "date", weekly: "date", monthly: "date" }
   }
   ```

### **Phase 3: Task Hiding Implementation**
1. **Hide/Show Logic**
   - Tasks completely removed from DOM when hidden
   - Progress calculation excludes hidden tasks
   - Section-level "Show Hidden" buttons

2. **User Interface**
   - Right-click context menu on tasks for hiding
   - "Show Hidden Tasks" button near collapse toggles
   - Settings in hamburger menu for global vs profile hiding

### **Phase 4: Global Auto-Reset**
1. **Cross-Profile Reset Logic**
   ```javascript
   function performGlobalReset(resetType) {
     const profiles = getAllProfiles();
     Object.keys(profiles).forEach(profileId => {
       resetProfileTasks(profileId, resetType);
     });
     updateLastResetCheck(resetType);
   }
   ```

2. **Enhanced Notifications**
   - Show number of profiles affected
   - List profile names in notification
   - Persist notification longer for visibility

## 🎨 Visual Design

### **Color Scheme Integration**
- Hamburger menu: Match existing `--primary-color` and `--secondary-color`
- Profile icon: Use `--success-color` for active state
- Hidden task indicators: Use `--accent-color` for visibility

### **Animation Specifications**
- Hamburger menu slide: 300ms ease-in-out
- Profile dropdown: 200ms ease-in-out
- Task hide/show: 250ms fade transition
- Backdrop fade: 200ms opacity transition

### **Responsive Breakpoints**
- Mobile (≤768px): Full-width hamburger menu
- Tablet (769px-1024px): Reduced menu width
- Desktop (≥1025px): Standard menu width (300px)

## 📱 Mobile Optimization

### **Touch Interactions**
- Larger touch targets for menu items
- Swipe gesture to close hamburger menu
- Long-press for task hiding context menu

### **Layout Adjustments**
- Profile icon shows only initial on mobile
- Hamburger menu items stack vertically
- Increased padding for touch accessibility

## 🔄 Migration Strategy

### **Data Migration**
1. **Existing Profiles**: Add default settings structure
2. **Current Progress**: Preserve all existing task states
3. **Legacy Support**: Maintain backward compatibility
4. **Graceful Degradation**: Fallback for missing data

### **User Experience Transition**
1. **First Load**: Show brief tutorial highlighting new features
2. **Settings Migration**: Automatically apply current density/column settings to active profile
3. **Progressive Enhancement**: New features don't break existing functionality

## ✅ Success Criteria

### **Functional Requirements**
- [ ] Hamburger menu contains all control functions
- [ ] Profile icon shows current profile and manages profiles
- [ ] Task hiding works both globally and per-profile
- [ ] Hidden tasks don't affect progress calculations
- [ ] Auto-resets apply to all profiles
- [ ] All settings persist across sessions

### **Performance Requirements**
- [ ] Menu animations are smooth (60fps)
- [ ] Task hiding/showing is instantaneous
- [ ] No performance degradation with large task lists
- [ ] Mobile responsiveness maintained

### **User Experience Requirements**
- [ ] Intuitive navigation and controls
- [ ] Clear visual feedback for all actions
- [ ] Consistent design language throughout
- [ ] Accessible keyboard navigation
- [ ] Touch-friendly mobile interface

This implementation plan provides a comprehensive roadmap for enhancing the RS3 Repeatables application with all requested features while maintaining the existing functionality and improving the overall user experience.