# Kanban Board 

Kanban Board allows users to create, manage, and organize tasks into customizable columns. Users can add new tasks, move them between columns (e.g., To Do, In Progress, Done) using drag-and-drop, and customize each column’s color. The app uses React for dynamic UI updates and local storage to persist data across sessions. This project demonstrates fundamental React concepts like state management, event handling, and working with local storage for persistence.

## Table of Contents
- [Technologies Used](#technologies-used)
- [Features](#features)
- [Development Stage](#development-stage)
- [Optimizations](#optimizations)
- [Lessons Learned](#lessons-learned)
- [Resources](#resources)
- [Contact](#contact)

## Technologies Used
Frontend
- React
- Vite
- TailwindCSS
- Javascript
- HTML

Backend
- Node.js
- Express
- MongoDB

Other Tools
- **Design:** Figma
- **Testing:** Postman
- **Version Control:** Git

## Features
- Create, Read, Update, and Delete Tasks
- Create multiple Kanban boards
- Drag-and-Drop: Move tasks between columns (e.g., To Do, In Progress, Done) using drag-and-drop functionality.
- Customizable Columns: Change the background color of each column to personalize the board.
- Persistence: Tasks and column data are saved in MongoDB, so users' progress is preserved.
- Secret Mode: Some secret color themes (To be revealed later)
- Mobile-first Design: The app is built to be responsive and works on both desktop and mobile devices.

## Development Stage
**Dec 8, 2024**
-Today, I integrated the backend with the React frontend, enabling the frontend to connect to the API in `server.js`. I used `create vite@latest` to quickly set up the boilerplate.
- I decided to transition from using vanilla MongoDB to **Mongoose**, as it provides a much cleaner and more efficient way to define schemas and models, especially when it comes to updating tasks, boards, and columns.
- I added functionality to create a new board with three default columns: **To Do**, **In Progress**, and **Done**. Additionally, I implemented a route at `/boards/title/:id` to allow users to update the board title.
- To build out the homepage, where users can either create a new board or select an existing one, I’m using **react-router-dom** to define two routes: **Homepage** and **BoardView**.
- Because I restructured the `App.jsx` component, I moved the `openModal` state into `BoardView` to ensure the modal is properly tied to the board view.
- I left off working on the feature that will allow users to select an existing board from the homepage. To make this happen, I need to create a route in `server.js` that returns a list of board titles, which I will then fetch and display dynamically on the homepage.

Next steps:
- Add ability to select one of your existing boards from homepage
- Style homepage
- Navigate Home > Board page after creating board
- Have boardview dynamically read/display title from database
- Give user ability to change title (using database)
- Consider changing endpoints to have query parameters (i.e., ?q=) instead of just longer and longer paths (how it is now). Model it off of apis like cocktailDB
- Integrate client and server readmes

**Dec 7, 2024** Progress
- Solved a problem with the `PATCH` request: The issue was not with matching the task ID in MongoDB Atlas but with handling `req.body` for the `status`. For now, I’ve hard-coded the status to `"In Progress"`. Once the frontend is implemented, I’ll update this to dynamically fetch the status from a form via `req.body`.
- Added functionality to update the following task properties:
  - **Status**
  - **Description**
  - **Title**

Currently, these updates are hard-coded, but they will eventually be dynamic with frontend input.
 Implemented Functionality
- **Read:** Retrieve all existing tasks.
- **Create:** Add new tasks.
- **Update:** Modify the task status, title, and description using `PATCH` requests.
- **Delete:** Delete a task by its ID in MongoDB.

In working on the delete functionality, I ran into an issue with parsing the id as an ObjectID correctly. While I am got a deprecation warning about new ObjectID(), I was able to use it to match up the ID from the query parameter with the __id in MongoDB.

## **Next Steps**
- Start implementing the frontend:
  - Begin integrating the **React** components to display and interact with the tasks from the backend.
  - Set up routing and state management (likely using React's `useState` and `useEffect` or a state management library like Redux) for task updates.
  - Connect the frontend with the backend endpoints to allow for dynamic interactions, such as adding, updating, and deleting tasks.
  
- Further refine backend functionality:
  - Update the `PATCH` endpoint to handle dynamic task status from the frontend rather than being hardcoded.
  - Implement additional validation and error handling for the `PATCH` and `DELETE` requests.
- Plan the database schema optimization for scalability as the project grows.

**Dec 6, 2024**
I have started to learn Node,js, Express, and MongoDB so I will be switching to working on the backend. Instead of local storage, I am building an API that interfaces with the database. I chose to begin the backend development from scratch as I am currently learning backend technologies and wanted a clean start before integrating them with the frontend. 
 Reflection on CRUD Functionality:
- **Create (POST):** Add tasks to the board.
- **Read (GET):** Display tasks on the board.
- **Update (PUT):** Edit task details.
- **Delete (DELETE):** Remove tasks.

This project will be my first full-stack app. I’ve decided to focus entirely on the backend first before switching to frontend development.

Next Steps
- Resolve `PATCH` request to update the task status (e.g., from "To Do" to "In Progress") by matching the task ID and updating the status.

**Nov 26, 2024:**
**Refactoring KanbanBoard component**
- To streamline the `KanbanBoard` component and improve readability:
  - Created a new `EditableTitle` component to handle the board title logic and UI.
  - Moved the `Column` components out of `KanbanBoard` and into `App.jsx`. These are now passed to `KanbanBoard` as `children` props.

**Saving Kanban title in local storage:**
- Used `useState` for the `boardTitle`. 
  - The state is initialized by checking if a title exists in `localStorage`. If not, it defaults to "My Kanban Board."
- Leveraged the `useEffect` hook to ensure that the board title in `localStorage` updates whenever the user modifies the title.
  - This approach keeps the local storage and app state in sync efficiently.

**Steps I'll take for adding tasks:**
1. Changed the "Add" button type to `submit`. ✅
2. Added state variables for task title and description. ✅
3. Linked the input fields (`textarea` and `text`) to state via `onChange` handlers. ✅
4. Wrapped the input fields in a `form` element and added an `onSubmit={handleSubmit}` handler. ✅
5. Implement `handleSubmit` to save tasks into an object and store them in `localStorage`. (TO DO NEXT TIME)

**Current Focus Areas**
- Dynamic input width for kanban title
- Finish implementing task state management

**Nov 24, 2024:**
- First, I decided to move AddNewTask from its own jsx file into Column.jsx since it was a small function that is only used in the Column component.
- I reread some of the React docs while I thought about how I want to pass in the various tasks/task cards into each column. I have decided to set a prop "children" for Column and then pass in the tasks object/task cards as the children. The tasks will conditionally render based on which column its supposed to be in. Using a children prop makes it so we don't need to know what will be inside each Column.

**Nov 23, 2024:**
**Event Handling & Modal State**
- Went back to the React docs to refamiliarize myself with event handling.
- I was still deciding where the best place is to store the `openModal` state for adding a new task.
- Realized I needed to add an Add/Close button to the new task form and style it as a modal.
- After considering different approaches, I decided that the `NewTaskForm` will be nested within the `App` component. Since the modal is a global feature in the app, the `openModal` state will live in `App`.
- The modal will be triggered by clicking "Add new task" and can be closed by clicking "cancel."
- Therefore, I’ll need to store both `openModal` and `setOpenModal` in `App` and pass them down as props to `NewTaskForm`.
- The `setOpenModal` function will then be passed through `KanbanBoard` > `Column` > `AddNewTask`.
- Plan to improve this by having the modal close by clicking outside of it and add an "Are you sure?" prompt to make sure they don't lose their work.

**6:21 PM - Editable Title**
- Decided to make the board title editable.
- Placed the title state in the `KanbanBoard` component.
- When the user is not editing, the title is displayed as a read-only `<h1>`.
- When the pencil icon is clicked, the title switches to an input field for editing.
- Currently, users can press **Enter** to submit the new title.
- Plan to improve this in the future by adding clearer submit and cancel actions (e.g., check and X buttons).

**Nov 21, 2024:**
- Started by creating a boilerplate React app using `create vite@latest`.

**Steps Taken:**
1. **Break UI into Component Hierarchy:**
   - Designed a wireframe of the MVP (Minimum Viable Product) for the Kanban board.
   - Broke the UI into a component hierarchy:
     - `App`
       - `KanbanBoard`
         - `ColorPicker`
         - `Column` (with title, color, tasks)
           - `TaskCard` (with task name, description)
         - `NewTaskForm`

2. **Build Static Version in React:**
   - Built a static version of the app top-down, starting with the layout and basic UI elements.
   - Added comments (*** in the code) to remind myself of areas to enhance, like adding more functionality or styling.

3. **Define the UI State:**
   - Identified the core data structure to manage the UI:
     - **Kanban board title**
     - **Array of columns** with properties: `id`, `title`, and `color`.
     - **Array of tasks** with `id`, `title`, `description`, and `status` (i.e., `toDo`, `inProgress`, `done`).
     - **Modal state** (`isModalOpen`) to control the visibility of the New Task Form.

**Next Steps**
- Dynamic kanban title input width
- Have modal close by clicking outside of it and add an "Are you sure?" prompt to make sure they don't lose their work.
- Implement functionality to add a task via the `NewTaskForm` modal when the plus button is clicked.
- Add state management for:
   - **Array of column objects** (id, title, color).
   - **Array of task objects** (id, title, description, status).
   - **Adding tasks to board**.
- Add drag and drop functionality
- Refine styling

## Optimizations
- Add styling and responsive design using **TailwindCSS**.
- Add a submit/cancel button for the title editing.
- Add color picker for each column background, put picker to right of column title
- Toggle dark/light/charcuterie/Y2k mode
- Add color picker to NewTaskForm so user can pick color of each task as well
- Add a secret easter egg charcuterie board mode. All the tasks will become cheese/boards will be wooden, etc.
- Add team-collaboration/assign tasks to a person
- Allow users to make more columns and rename the columns
- Add user authentication/login

## Lessons Learned
- Re-familiarized myself with the basics of **React**.
- Gained experience in breaking down a project into components and understanding how to structure the app for better maintainability.
- When I initially began building the Kanban board app using **React**, I had no experience with backend development. Now, after learning backend technologies, I’ve decided to start fresh and focus on building the backend first and integrate it with the frontend later.
- I made a key mistake of forgetting to put a forward slash at the beginning of an endpoint-- will not make this one again!
 Notes:
- Any comments in the code prefixed with three stars (**`***`**) indicate areas that need further implementation or revisiting.

## Resources
- [Tailwind Docs](https://tailwindcss.com/docs/)
- [React Icons](https://react-icons.github.io/react-icons/)
- [React Docs](https://react.dev/)
- [Mongoose Docs](https://mongoosejs.com/docs/guide.html)

## Contact
<p> 
  <a href="https://raisadorzback.netlify.app/" target="blank">
    <img src="https://img.shields.io/badge/Website-563d7c?&style=for-the-badge" alt="Website">
  </a>
  <a href="https://www.linkedin.com/in/raisa-d/">
    <img src="https://img.shields.io/badge/LinkedIn-046E6D?logo=linkedin&style=for-the-badge">
  </a>
  <a href="https://twitter.com/rai__bread" target="blank">
    <img src="https://img.shields.io/badge/Twitter-563d7c?logo=twitter&style=for-the-badge&logoColor=white" alt="rai__bread" />
  </a> 
</p>