# LaTeX Report Prompt Generator

A Vue 3 application that helps generate structured ChatGPT prompts for creating LaTeX design reports. The app provides a user-friendly interface to input project details and generates a well-formatted prompt that instructs ChatGPT to create a LaTeX document.

## Features

- Dark-themed, responsive UI built with Tailwind CSS
- Input fields for project title, problem statement, constraints, and criteria
- Dynamic management of design options and evaluation criteria
- Automatic prompt generation for ChatGPT
- Copy to clipboard functionality
- Export prompt to text file

## Setup

1. Install dependencies:

```bash
npm install
```

2. Start the development server:

```bash
npm run dev
```

3. Build for production:

```bash
npm run build
```

## Usage

1. Fill in the project details:

   - Enter project title
   - Describe the problem statement
   - Add constraints using the "Add Constraint" button
   - Add evaluation criteria and their weights
   - Add design options and score them against each criterion

2. The prompt will be automatically generated as you input data

3. Use the generated prompt:
   - Click "Copy to Clipboard" to copy the prompt
   - Click "Export to File" to save the prompt as a text file
   - Paste the prompt into ChatGPT to generate your LaTeX report

## Technology Stack

- Vue 3 (Composition API)
- Vite
- Tailwind CSS
- Modern JavaScript (ES6+)
