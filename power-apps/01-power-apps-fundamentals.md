# Power Apps Fundamentals

**Date:** January 2026  
**Author:** Power Platform Journey  
**Category:** Power Apps  
**Tags:** #power-apps #canvas-apps #low-code #app-development

## Overview

Power Apps enables rapid application development with minimal code. This guide covers fundamental concepts and best practices for building effective business applications.

## Understanding Power Apps

Power Apps offers two primary app types:

### Canvas Apps
- **Design Approach**: Start with a blank canvas
- **Control Level**: Pixel-perfect control over layout
- **Data Sources**: Connect to multiple sources
- **Use Cases**: Task-specific apps, mobile solutions

### Model-Driven Apps
- **Design Approach**: Start with data model (Dataverse)
- **Control Level**: Standardized interface
- **Data Sources**: Primarily Dataverse
- **Use Cases**: Complex business processes, form-centric apps

## Creating Your First Canvas App

### Step 1: Choose Your Data Source

Common options:
- SharePoint lists
- Excel files (OneDrive/SharePoint)
- Dataverse
- SQL Server
- Custom APIs

### Step 2: Design the Interface

Key screens in most apps:
1. **Browse Screen** - List of items
2. **Detail Screen** - View single item
3. **Edit/New Screen** - Create or modify items

### Step 3: Add Functionality

Basic formulas you'll use often:
```
// Navigate between screens
Navigate(DetailScreen, ScreenTransition.Fade)

// Filter a gallery
Filter(DataSource, Status = "Active")

// Submit a form
SubmitForm(EditForm1)

// Show notification
Notify("Record saved successfully", NotificationType.Success)
```

## Core Concepts

### Controls
Building blocks of your app:
- **Input**: Text, Date picker, Dropdown, Combo box
- **Display**: Label, Image, Gallery, Data table
- **Button**: Execute actions
- **Forms**: Edit and display data

### Formulas
Power Apps uses Excel-like formulas:
```
// Basic operations
Text(Value, "[$-en-US]$#,##0.00")
DateDiff(StartDate, EndDate, Days)
Concatenate(FirstName, " ", LastName)

// Collections
Collect(TempData, {Name: "John", Age: 30})
ClearCollect(FilteredData, Filter(AllData, Active))

// Conditional logic
If(Amount > 1000, "High", If(Amount > 100, "Medium", "Low"))
```

### Context and Variables
- **Global Variables**: Available throughout the app
  ```
  Set(CurrentUser, User().Email)
  ```
- **Context Variables**: Scoped to a screen
  ```
  UpdateContext({IsVisible: true})
  ```
- **Collections**: In-memory tables
  ```
  ClearCollect(MyData, DataSource)
  ```

## Design Best Practices

### 1. Consistent Naming
```
// Good naming conventions
btn_Submit, gal_Products, lbl_Title, frm_EditUser
txt_SearchBox, drp_Category, img_Logo
```

### 2. Responsive Design
```
// Use responsive containers and formulas
Width = Parent.Width * 0.9
Height = If(App.Height < 768, 300, 400)
```

### 3. Component Libraries
Create reusable components:
- Standard buttons
- Header/footer templates
- Custom navigation
- Data display cards

### 4. Performance Optimization
- **Delegation**: Understand data source limits
- **Caching**: Use collections for frequently accessed data
- **Lazy Loading**: Load data only when needed
- **Minimize Controls**: Limit controls per screen (<200)

## Real-World Example: Expense Tracker App

### Requirements
- Submit expense requests
- Attach receipts
- Track approval status
- Manager dashboard

### Implementation
```
// Home Screen Gallery
Items: Sort(Filter(Expenses, 
  Requestor = User().Email),
  SubmitDate, 
  Descending)

// Submit Button
OnSelect: 
  Patch(Expenses, Defaults(Expenses), {
    Title: txt_Title.Text,
    Amount: Value(txt_Amount.Text),
    Category: drp_Category.Selected.Value,
    Requestor: User().Email,
    Status: "Pending",
    SubmitDate: Now()
  });
  Navigate(HomeScreen, ScreenTransition.None);
  Notify("Expense submitted", NotificationType.Success)
```

### Challenges & Solutions

**Challenge**: Photo capture on mobile  
**Solution**: Used Camera control with base64 encoding

**Challenge**: Offline capability  
**Solution**: SaveData/LoadData for local caching

## Common Patterns

### Master-Detail Pattern
```
// Gallery (Master)
OnSelect: 
  UpdateContext({SelectedItem: ThisItem});
  Navigate(DetailScreen)

// Detail Screen
Item: SelectedItem
```

### Search and Filter
```
Items: 
  Search(
    Filter(DataSource, 
      Status = drp_StatusFilter.Selected.Value),
    txt_SearchBox.Text,
    "Title", "Description"
  )
```

### Cascading Dropdowns
```
// First dropdown
Items: Distinct(Products, Category)

// Second dropdown (depends on first)
Items: Filter(Products, 
  Category = drp_Category.Selected.Value)
```

## Dataverse Integration

Benefits of using Dataverse:
- **Relationships**: Connect related data
- **Business Rules**: Server-side validation
- **Security**: Role-based access control
- **Offline**: Built-in offline support

Basic Dataverse operations:
```
// Create record
Patch(Accounts, Defaults(Accounts), {
  Name: "Contoso",
  Revenue: 1000000
})

// Update record
Patch(Accounts, LookUp(Accounts, Name = "Contoso"), {
  Revenue: 1500000
})

// Relate records
Patch(Contacts, Defaults(Contacts), {
  'Account': LookUp(Accounts, Name = "Contoso")
})
```

## Testing and Deployment

### Testing Checklist
- [ ] Test on different devices (mobile, tablet, desktop)
- [ ] Verify all navigation paths
- [ ] Test error scenarios
- [ ] Validate data operations (CRUD)
- [ ] Check performance with production data volume
- [ ] Test with different user roles

### Deployment
1. Share with users/groups
2. Set permissions appropriately
3. Monitor analytics
4. Gather user feedback
5. Iterate and improve

## Next Steps

- [Canvas App Design Patterns](./02-canvas-app-design-patterns.md)
- [Working with Dataverse](./03-working-with-dataverse.md)

## Resources

- [Power Apps Documentation](https://learn.microsoft.com/en-us/power-apps/)
- [Formula Reference](https://learn.microsoft.com/en-us/power-platform/power-fx/formula-reference)
- [Canvas Apps Training](https://learn.microsoft.com/en-us/training/paths/create-powerapps/)

---

*What's your favorite Power Apps tip? Share in the discussions!*
