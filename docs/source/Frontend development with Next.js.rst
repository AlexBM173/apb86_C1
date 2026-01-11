15. Frontend development with Next.js
=====================================

The structure of our full stack application will look like this:

.. code-block:: text

   my_full_stack_app/
   ├── backend/                 # Your FastAPI backend
   │   └── main.py
   |   └── requirements.txt
   └── frontend/                # Your Next.js frontend
       ├── app/
       ├── package.json

The frontend of a web application is the part that users interact with. HTML, CSS, and JavaScript are the main technologies for making visually
appealing and interactive web pages. Next.js is a popular React framework that simplifies building server-side rendered and statically generated 
web applications.

1. Next.js project generator
----------------------------

You can scaffold a new read-to-run Next.js project using the following command:

.. code-block:: bash

   npx create-next-app@latest frontend

To add TypeScript, ESLint and Tailwind CSS, you can instead run:

.. code-block:: bash

   npx create-next-app@latest frontend --ts --eslint --tailwind

That new file structure will look like this:

.. code-block:: text

   frontend/
   ├── public/                  # Static assets
   ├── src/                     # Source files
   ├── node_modules/            # Project dependencies
   ├── README.md                # Project documentation
   ├── next-env.d.ts            # Next.js environment types
   ├── next.config.ts           # Next.js configuration
   ├── package.json             # Project metadata and dependencies
   ├── package-lock.json        # Lockfile for dependencies
   ├── tsconfig.json            # TypeScript configuration
   ├── eslint.config.mjs        # ESLint configuration
   └── next.config.ts           # Next.js configuration

You can run the development server with:

.. code-block:: bash

   cd frontend
   npm run dev

This starts the server on `http://localhost:3000`. We now need to call our backend from the frontend. To do this, in our ``main.py`` file in the
backend directory, you need to enable CORS:

.. code-block:: python

   from fastapi.middleware.cors import CORSMiddleware

   app.add_middleware(
       CORSMiddleware,
       allow_origins=["http://localhost:3000"],  # Frontend URL
       allow_credentials=True,
       allow_methods=["*"],
       allow_headers=["*"],
   )

You then need to add an environment variable to your frontend to point to the backend API. Create a file called
``.env.local`` in the ``frontend`` directory with the following content:

.. code-block:: text

   NEXT_PUBLIC_API_URL=http://localhost:8000

You can then access this variable in your frontend code using ``process.env.NEXT_PUBLIC_API_URL``. To kill a process running on a port, you can use

.. code-block:: bash

   kill <PID>  # or 8000 for the backend
   # or, if it won't die
   kill -9 <PID>

2. TypeScript
-------------

TypeScript is a high-level programming language that builds on JavaScript by adding static typing with optional type annotations. It is designed 
for the development of large applications and transpiles to JavaScript. TypeScript offers several advantages over JavaScript, including:

- Type signatures, which defines the inputs and outputs of functions.
- Type inference, which automatically deduces types when they are not explicitly defined.
- Interfaces, which define the structure of objects.
- Enumerated types, which define a set of ordered named constants.
- Generics, which enable the creation of reusable components.
- Namespaces, which organise code into logical groups.
- Tuples, which define arrays with fixed sizes and types.
- Explicit resource management, which helps manage memory and other resources.

3. ESLint
---------

ESLint is a static code analysis tool for JavaScript and TypeScript. It helps identify and fix errors in your code, bad practices, and style
incosistencies. It is a configurable tool that allows you to customise rules and automate fixes.

4. Tailwind CSS
---------------

Tailwind CSS is a cascading style sheet (CSS) framework that provides a list of utility classes, as opposed to creating classes around components 
(buttons, panels, cards, etc). Some utility classes include:

- ``bg-blue-500``: Sets the background color to blue.
- ``text-white``: Sets the text color to white.
- ``p-4``: Adds padding of 1rem (16px) on all sides.
- ``m-2``: Adds margin of 0.5rem (8px) on all sides.
- ``flex``: Applies flexbox layout to an element.
- ``grid``: Applies grid layout to an element.

The basic structure of a Tailwind CSS colour class is ``{property}-{color}-{shade}``, where:

- ``property``: The CSS property being modified (e.g., bg for background, text for text color).
- ``color``: The color name (e.g., blue, red, green).
- ``shade``: The shade of the color, typically a number from 100 to 900, where lower numbers are lighter and higher numbers are darker.