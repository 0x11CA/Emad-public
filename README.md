i want you to write it in a readme file: well organized and easy to understand:
# Emad Platform Database Visual Diagram

## 🏗️ Entity Relationship Diagram (ASCII)

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                              EMAD PLATFORM DATABASE                            │
└─────────────────────────────────────────────────────────────────────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│   DIRECTORATE   │         │    DEPARTMENT   │         │      USER       │
│                 │         │                 │         │                 │
│ _id: ObjectId   │◄────────┤ directorate     │         │ _id: ObjectId   │
│ name: String    │         │ _id: ObjectId   │         │ username: String│
│ code: String    │         │ name: String    │         │ email: String   │
│ description:    │         │ code: String    │         │ password: String│
│ String          │         │ description:    │         │ firstName: String│
│ director:       │         │ String          │         │ lastName: String │
│ ObjectId        │         │ director:       │         │ role: String     │
│ departments:    │         │ ObjectId        │         │ directorate:     │
│ [ObjectId]      │         │ staff: [ObjectId]│        │ ObjectId        │
│ isActive:       │         │ isActive:       │         │ department:     │
│ Boolean         │         │ Boolean         │         │ ObjectId        │
│ contactInfo:    │         │ capacity: Number│         │ position: String│
│ Object          │         │ specialties:    │         │ employeeId:     │
│ budget: Object  │         │ [String]        │         │ String          │
└─────────────────┘         │ contactInfo:    │         │ phoneNumber:    │
                            │ Object          │         │ String          │
                            └─────────────────┘         │ isActive:       │
                                                        │ Boolean         │
                                                        │ emailVerified:  │
                                                        │ Boolean         │
                                                        │ isFirstLogin:   │
                                                        │ Boolean         │
                                                        │ isTemporaryPassword: Boolean
                                                        └─────────────────┘
                                                                 │
                                                                 │
                            ┌─────────────────┐         ┌─────────────────┐
                            │      EVENT      │         │      TASK       │
                            │                 │         │                 │
                            │ _id: ObjectId   │         │ _id: ObjectId   │
                            │ title: String   │         │ title: String   │
                            │ description:    │         │ description:    │
                            │ String          │         │ String          │
                            │ shortDescription│         │ event: ObjectId │
                            │ category: String│         │ assignedTo:     │
                            │ date: Date      │         │ ObjectId        │
                            │ endDate: Date   │         │ assignedBy:     │
                            │ location: String│         │ ObjectId        │
                            │ projectManager: │         │ department:     │
                            │ ObjectId        │         │ String          │
                            │ departmentReqs: │         │ priority: String│
                            │ [Object]        │         │ status: String  │
                            │ status: String  │         │ dueDate: Date   │
                            │ isPublic:       │         │ parentTask:     │
                            │ Boolean         │         │ ObjectId        │
                            │ budget: Object  │         │ subtasks:       │
                            │ contactInfo:    │         │ [ObjectId]      │
                            │ Object          │         │ dependencies:   │
                            │ highlights:     │         │ [ObjectId]      │
                            │ [String]        │         │ comments:       │
                            │ tags: [String]  │         │ [Object]        │
                            │ registrationReq:│         │ progress: Number│
                            │ Boolean         │         │ timeSpent:      │
                            │ maxAttendees:   │         │ Number          │
                            │ Number          │         └─────────────────┘
                            └─────────────────┘

```

## 🔗 Relationship Types

### **One-to-Many (1:N)**
```
Directorate ──1:N──► Department
Department ──1:N──► Staff (Users)
Event ──1:N──► Tasks
Task ──1:N──► Subtasks
```

### **Many-to-One (N:1)**
```
Department ──N:1──► Directorate
User ──N:1──► Directorate (for directors)
User ──N:1──► Department (for staff)
Task ──N:1──► Event
Task ──N:1──► Assigned User
```

### **Many-to-Many (M:N)**
```
Events ↔ Departments (via departmentRequirements)
Tasks ↔ Users (via assignments over time)
```

## 🎯 Role Hierarchy

```
                    ADMIN
                      │
                      ├── DIRECTOR
                      │     │
                      │     ├── DEPARTMENT_HEAD
                      │     │     │
                      │     │     └── STAFF
                      │     │
                      │     └── PROJECT_MANAGER
                      │           │
                      │           └── TEAM_LEADER
                      │
                      ├── AUDITOR
                      ├── EXTERNAL_PARTNER
                      └── EXECUTIVE
```

## 📊 Data Flow Examples

### **Event Creation Flow:**
```
1. PROJECT_MANAGER creates event
2. DEPARTMENT_HEADS receive approval requests
3. DEPARTMENT_HEADS approve and assign staff
4. TEAM_LEADERS are assigned
5. STAFF are assigned to tasks
6. Tasks are created and tracked
```

### **Staff Assignment Flow:**
```
1. DIRECTOR manages departments
2. DEPARTMENT_HEAD manages staff
3. PROJECT_MANAGER requests staff
4. DEPARTMENT_HEAD approves assignments
5. STAFF receive notifications
```

## 🛡️ Key Constraints

### **Unique Constraints:**
- Directorate codes
- Department codes  
- User emails
- User usernames
- Employee IDs

### **Referential Integrity:**
- Department → Directorate
- User → Directorate/Department
- Task → Event/User
- Event → Project Manager

### **Business Rules:**
- Directors manage one directorate
- Department heads manage one department
- Staff belong to one department
- Events follow status workflow
- Tasks follow assignment rules 




# Emad Platform Database Structure

## 🏗️ Database Architecture Overview

The Emad platform uses MongoDB with Mongoose ODM to manage a hierarchical organizational structure for the Ministry of Culture, Sports and Youth in Oman.

---

## 📊 Entity Relationship Diagram

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│    DIRECTORATE  │    │    DEPARTMENT   │    │      USER       │
│                 │    │                 │    │                 │
│ _id: ObjectId   │◄───┤ directorate     │    │ _id: ObjectId   │
│ name: String    │    │ _id: ObjectId   │    │ username: String│
│ code: String    │    │ name: String    │    │ email: String   │
│ description:    │    │ code: String    │    │ password: String│
│ String          │    │ description:    │    │ firstName: String│
│ director:       │    │ String          │    │ lastName: String │
│ ObjectId        │    │ director:       │    │ role: String     │
│ departments:    │    │ ObjectId        │    │ directorate:     │
│ [ObjectId]      │    │ staff: [ObjectId]│   │ ObjectId        │
│ isActive:       │    │ isActive:       │    │ department:     │
│ Boolean         │    │ Boolean         │    │ ObjectId        │
│ contactInfo:    │    │ capacity: Number│    │ position: String│
│ Object          │    │ specialties:    │    │ employeeId:     │
│ budget: Object  │    │ [String]        │    │ String          │
└─────────────────┘    │ contactInfo:    │    │ phoneNumber:    │
                       │ Object          │    │ String          │
                       └─────────────────┘    │ isActive:       │
                                              │ Boolean         │
                                              │ emailVerified:  │
                                              │ Boolean         │
                                              │ isFirstLogin:   │
                                              │ Boolean         │
                                              │ isTemporaryPassword: Boolean
                                              └─────────────────┘
                                                       │
                                                       │
                       ┌─────────────────┐    ┌─────────────────┐
                       │      EVENT      │    │      TASK       │
                       │                 │    │                 │
                       │ _id: ObjectId   │    │ _id: ObjectId   │
                       │ title: String   │    │ title: String   │
                       │ description:    │    │ description:    │
                       │ String          │    │ String          │
                       │ shortDescription│    │ String          │
                       │ category: String│    │ event: ObjectId │
                       │ date: Date      │    │ assignedTo:     │
                       │ endDate: Date   │    │ ObjectId        │
                       │ location: String│    │ assignedBy:     │
                       │ projectManager: │    │ ObjectId        │
                       │ ObjectId        │    │ department:     │
                       │ departmentReqs: │    │ String          │
                       │ [Object]        │    │ priority: String│
                       │ status: String  │    │ status: String  │
                       │ isPublic:       │    │ dueDate: Date   │
                       │ Boolean         │    │ parentTask:     │
                       │ budget: Object  │    │ ObjectId        │
                       │ contactInfo:    │    │ subtasks:       │
                       │ Object          │    │ [ObjectId]      │
                       │ highlights:     │    │ dependencies:   │
                       │ [String]        │    │ [ObjectId]      │
                       │ tags: [String]  │    │ comments:       │
                       │ registrationReq:│    │ [Object]        │
                       │ Boolean         │    │ progress: Number│
                       │ maxAttendees:   │    │ timeSpent:      │
                       │ Number          │    │ Number          │
                       └─────────────────┘    └─────────────────┘
```

---

## 📋 Detailed Schema Definitions

### 1. **DIRECTORATE Collection**

```javascript
{
  _id: ObjectId,
  name: String,           // "Directorate of Cultural Affairs"
  code: String,           // "DCA" (unique, uppercase)
  description: String,    // "Manages cultural events and heritage"
  director: ObjectId,     // Reference to User (role: 'director')
  departments: [ObjectId], // Array of Department ObjectIds
  isActive: Boolean,      // true/false
  contactInfo: {
    email: String,        // "dca@mcsy.gov.om"
    phone: String,        // "+968 1234 5678"
    office: String        // "Building A, Floor 2"
  },
  budget: {
    allocated: Number,    // 500000
    spent: Number,        // 0
    currency: String      // "OMR"
  },
  createdAt: Date,
  updatedAt: Date
}
```

### 2. **DEPARTMENT Collection**

```javascript
{
  _id: ObjectId,
  name: String,           // "Heritage Preservation Department"
  code: String,           // "HPD" (unique, uppercase)
  description: String,    // "Manages national heritage sites"
  directorate: ObjectId,  // Reference to Directorate
  director: ObjectId,     // Reference to User (role: 'department_head')
  staff: [ObjectId],      // Array of User ObjectIds (role: 'staff')
  isActive: Boolean,      // true/false
  capacity: Number,       // 15 (max staff capacity)
  specialties: [String],  // ["Heritage Management", "Cultural Preservation"]
  contactInfo: {
    email: String,        // "hpd@mcsy.gov.om"
    phone: String,        // "+968 1234 5682"
    office: String        // "Building A, Floor 2, Room 201"
  },
  createdAt: Date,
  updatedAt: Date
}
```

### 3. **USER Collection**

```javascript
{
  _id: ObjectId,
  username: String,       // "director_dca" (unique)
  email: String,          // "director.dca@mcsy.gov.om" (unique)
  password: String,       // Hashed with bcrypt
  firstName: String,      // "Director"
  lastName: String,       // "Cultural"
  role: String,           // Enum: ['admin', 'director', 'department_head', 'project_manager', 'team_leader', 'staff', 'auditor', 'external_partner', 'executive']
  directorate: ObjectId,  // Reference to Directorate (for directors & department_heads)
  department: ObjectId,   // Reference to Department (for department_heads & staff)
  position: String,       // "Director"
  employeeId: String,     // "DIRDCA001" (unique)
  phoneNumber: String,    // "+968123456789"
  profilePicture: String, // URL to profile image
  isActive: Boolean,      // true/false
  emailVerified: Boolean, // true/false
  isFirstLogin: Boolean,  // true/false
  isTemporaryPassword: Boolean, // true/false
  createdAt: Date,
  updatedAt: Date
}
```

### 4. **EVENT Collection**

```javascript
{
  _id: ObjectId,
  title: String,          // "National Heritage Exhibition 2024"
  description: String,    // "Annual cultural heritage showcase"
  shortDescription: String, // "Heritage exhibition"
  category: String,       // Enum: ['conference', 'workshop', 'exhibition', 'ceremony', 'meeting', 'training', 'other']
  date: Date,             // Start date and time
  endDate: Date,          // End date and time
  location: String,       // "National Museum, Muscat"
  projectManager: ObjectId, // Reference to User (role: 'project_manager')
  departmentRequirements: [{
    department: String,   // Enum: ['media', 'logistics', 'technical', 'security', 'catering', 'registration', 'transportation', 'other']
    staffCount: Number,   // 5
    needsLeader: Boolean, // true
    description: String,  // "Media coverage and documentation"
    status: String,       // Enum: ['pending', 'approved', 'rejected']
    assignedStaff: [ObjectId], // Array of User ObjectIds
    teamLeader: ObjectId  // Reference to User (role: 'team_leader')
  }],
  status: String,         // Enum: ['draft', 'pending_approval', 'approved', 'in_progress', 'completed', 'cancelled']
  isPublic: Boolean,      // true/false
  budget: {
    amount: Number,       // 50000
    currency: String      // "OMR"
  },
  contactInfo: {
    email: String,        // "contact@heritage.gov.om"
    phone: String,        // "+968 1234 5678"
    website: String       // "https://heritage.gov.om"
  },
  highlights: [String],   // ["Live demonstrations", "Interactive exhibits"]
  tags: [String],        // ["heritage", "culture", "exhibition"]
  registrationRequired: Boolean, // true/false
  registrationDeadline: Date,    // Registration deadline
  maxAttendees: Number,          // 1000
  createdAt: Date,
  updatedAt: Date
}
```

### 5. **TASK Collection**

```javascript
{
  _id: ObjectId,
  title: String,          // "Set up exhibition booths"
  description: String,    // "Install and arrange exhibition displays"
  event: ObjectId,        // Reference to Event
  assignedTo: ObjectId,   // Reference to User (role: 'staff')
  assignedBy: ObjectId,   // Reference to User (role: 'team_leader' or 'project_manager')
  department: String,     // Enum: ['media', 'logistics', 'technical', 'security', 'catering', 'registration', 'transportation', 'other']
  priority: String,       // Enum: ['low', 'medium', 'high', 'urgent']
  status: String,         // Enum: ['pending', 'in_progress', 'completed', 'cancelled']
  dueDate: Date,          // Task deadline
  parentTask: ObjectId,   // Reference to parent Task (for subtasks)
  subtasks: [ObjectId],   // Array of Task ObjectIds (subtasks)
  dependencies: [ObjectId], // Array of Task ObjectIds (dependencies)
  comments: [{
    user: ObjectId,       // Reference to User
    content: String,      // Comment text
    createdAt: Date       // Comment timestamp
  }],
  progress: Number,       // 0-100 (percentage)
  timeSpent: Number,      // Hours spent on task
  createdAt: Date,
  updatedAt: Date
}
```

---

## 🔗 Relationship Mappings

### **One-to-Many Relationships:**

1. **Directorate → Departments**
   - One directorate can have multiple departments
   - Each department belongs to exactly one directorate

2. **Directorate → Director (User)**
   - One directorate has exactly one director
   - Each director can manage only one directorate

3. **Department → Department Head (User)**
   - One department has exactly one department head
   - Each department head can manage only one department

4. **Department → Staff (Users)**
   - One department can have multiple staff members
   - Each staff member belongs to exactly one department

5. **Event → Project Manager (User)**
   - One event has exactly one project manager
   - Each project manager can manage multiple events

6. **Task → Assigned User**
   - One task is assigned to exactly one user
   - Each user can have multiple assigned tasks

### **Many-to-Many Relationships:**

1. **Events ↔ Departments** (via departmentRequirements)
   - Events can request staff from multiple departments
   - Departments can provide staff for multiple events

2. **Tasks ↔ Users** (via assignments)
   - Tasks can be assigned to different users over time
   - Users can work on multiple tasks

---

## 🎯 Role-Based Access Control (RBAC)

### **Role Hierarchy:**

```
ADMIN (Platform Manager)
├── DIRECTOR (Directorate Director)
│   ├── DEPARTMENT_HEAD (Department Head)
│   │   └── STAFF (Employee)
│   └── PROJECT_MANAGER (Event Creator)
│       └── TEAM_LEADER (Task Manager)
├── AUDITOR (Monitoring Unit)
├── EXTERNAL_PARTNER (Contractor)
└── EXECUTIVE (Ministerial Viewer)
```

### **Permission Matrix:**

| Role | Directorate Access | Department Access | Event Access | Task Access |
|------|-------------------|-------------------|--------------|-------------|
| **ADMIN** | Full (All) | Full (All) | Full (All) | Full (All) |
| **DIRECTOR** | Own Only | Own Directorate | Own Events | Own Events |
| **DEPARTMENT_HEAD** | Own Directorate | Own Department | Own Events | Own Department |
| **PROJECT_MANAGER** | None | None | Own Events | Own Events |
| **TEAM_LEADER** | None | None | Assigned Events | Assigned Tasks |
| **STAFF** | None | Own Department | Assigned Events | Assigned Tasks |
| **AUDITOR** | Read Only | Read Only | Read Only | Read Only |
| **EXTERNAL_PARTNER** | None | None | Limited | Limited |
| **EXECUTIVE** | Read Only | Read Only | Read Only | Read Only |

---

## 📊 Database Indexes

### **Performance Indexes:**

```javascript
// Directorate indexes
directorateSchema.index({ isActive: 1 });
directorateSchema.index({ director: 1 });
directorateSchema.index({ code: 1 });

// Department indexes
departmentSchema.index({ directorate: 1, isActive: 1 });
departmentSchema.index({ director: 1 });
departmentSchema.index({ code: 1 });

// User indexes
userSchema.index({ email: 1 });
userSchema.index({ username: 1 });
userSchema.index({ role: 1 });
userSchema.index({ directorate: 1 });
userSchema.index({ department: 1 });

// Event indexes
eventSchema.index({ projectManager: 1 });
eventSchema.index({ status: 1 });
eventSchema.index({ date: 1 });
eventSchema.index({ category: 1 });

// Task indexes
taskSchema.index({ event: 1 });
taskSchema.index({ assignedTo: 1 });
taskSchema.index({ status: 1 });
taskSchema.index({ dueDate: 1 });
```

---

## 🔄 Data Flow Examples

### **Event Creation Flow:**

1. **Project Manager** creates event with department requirements
2. **Department Heads** receive approval requests
3. **Department Heads** approve and assign staff
4. **Team Leaders** are assigned to manage tasks
5. **Staff** are assigned to specific tasks
6. **Tasks** are created and tracked

### **Staff Assignment Flow:**

1. **Director** manages directorate departments
2. **Department Head** manages department staff
3. **Project Manager** requests staff for events
4. **Department Head** approves and assigns staff
5. **Staff** are notified and can view assignments

---

## 🛡️ Data Validation Rules

### **Business Rules:**

1. **Unique Constraints:**
   - Directorate codes must be unique
   - Department codes must be unique
   - User emails must be unique
   - User usernames must be unique
   - Employee IDs must be unique

2. **Referential Integrity:**
   - Department must belong to a valid directorate
   - User directorate/department references must be valid
   - Event project manager must be a valid user
   - Task assignments must reference valid users

3. **Role Constraints:**
   - Directors can only manage one directorate
   - Department heads can only manage one department
   - Staff can only belong to one department
   - Project managers must have project_manager role

4. **Status Transitions:**
   - Events: draft → pending_approval → approved → in_progress → completed
   - Tasks: pending → in_progress → completed
   - Departments: Only active departments can be assigned to events

---

## 📈 Scalability Considerations

### **Horizontal Scaling:**
- MongoDB sharding by directorate
- Read replicas for reporting
- Separate collections for high-volume data

### **Performance Optimization:**
- Aggregation pipelines for complex queries
- Caching for frequently accessed data
- Background jobs for data processing

### **Data Archiving:**
- Archive completed events after 2 years
- Archive completed tasks after 1 year
- Maintain audit trails for compliance

---

This database structure provides a robust foundation for the Emad platform, supporting the hierarchical organizational structure of the Ministry while maintaining data integrity and performance.
