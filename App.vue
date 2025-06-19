<!-- App.vue -->
<template>
  <div class="min-h-screen bg-gray-900 text-gray-100 py-8">
    <div class="container mx-auto max-w-4xl px-4">
      <h1 class="text-3xl font-bold text-center mb-8">
        LaTeX Report Prompt Generator
      </h1>

      <!-- Project Title -->
      <div class="mb-6">
        <label class="block text-sm font-medium mb-2">Project Title</label>
        <input
          v-model="title"
          type="text"
          class="w-full bg-gray-800 border border-gray-700 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
          placeholder="Enter project title"
        />
      </div>

      <!-- Problem Statement -->
      <div class="mb-6">
        <label class="block text-sm font-medium mb-2">Problem Statement</label>
        <textarea
          v-model="problemStatement"
          rows="4"
          class="w-full bg-gray-800 border border-gray-700 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
          placeholder="Describe the problem to be solved"
        ></textarea>
      </div>

      <!-- Constraints -->
      <div class="mb-6">
        <div class="flex justify-between items-center mb-2">
          <label class="text-sm font-medium">Constraints</label>
          <button
            @click="addConstraint"
            class="px-3 py-1 bg-blue-600 text-sm rounded-lg hover:bg-blue-700 transition"
          >
            Add Constraint
          </button>
        </div>
        <div
          v-for="(constraint, index) in constraints"
          :key="index"
          class="flex gap-2 mb-2"
        >
          <input
            v-model="constraints[index]"
            type="text"
            class="flex-1 bg-gray-800 border border-gray-700 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
            placeholder="Enter constraint"
          />
          <button
            @click="removeConstraint(index)"
            class="px-3 py-1 bg-red-600 text-sm rounded-lg hover:bg-red-700 transition"
          >
            Remove
          </button>
        </div>
      </div>

      <!-- Criteria -->
      <div class="mb-6">
        <div class="flex justify-between items-center mb-2">
          <label class="text-sm font-medium">Criteria</label>
          <button
            @click="addCriterion"
            class="px-3 py-1 bg-blue-600 text-sm rounded-lg hover:bg-blue-700 transition"
          >
            Add Criterion
          </button>
        </div>
        <div
          v-for="(criterion, index) in criteria"
          :key="index"
          class="flex gap-2 mb-2"
        >
          <input
            v-model="criterion.name"
            type="text"
            class="flex-1 bg-gray-800 border border-gray-700 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
            placeholder="Criterion name"
          />
          <input
            v-model.number="criterion.weight"
            type="number"
            min="0"
            max="10"
            class="w-24 bg-gray-800 border border-gray-700 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
            placeholder="Weight"
          />
          <button
            @click="removeCriterion(index)"
            class="px-3 py-1 bg-red-600 text-sm rounded-lg hover:bg-red-700 transition"
          >
            Remove
          </button>
        </div>
      </div>

      <!-- Design Options -->
      <div class="mb-6">
        <div class="flex justify-between items-center mb-2">
          <label class="text-sm font-medium">Design Options</label>
          <button
            @click="addDesignOption"
            class="px-3 py-1 bg-blue-600 text-sm rounded-lg hover:bg-blue-700 transition"
          >
            Add Option
          </button>
        </div>

        <!-- Design Options List -->
        <div class="space-y-4 mb-4">
          <div
            v-for="(option, index) in designOptions"
            :key="index"
            class="bg-gray-800 border border-gray-700 rounded-lg p-4"
          >
            <div class="flex justify-between items-center mb-3">
              <h4 class="text-sm font-medium text-blue-400">
                Option {{ String.fromCharCode(65 + index) }}
              </h4>
              <button
                @click="removeDesignOption(index)"
                class="px-3 py-1 bg-red-600 text-sm rounded-lg hover:bg-red-700 transition"
              >
                Remove Option
              </button>
            </div>

            <div class="grid grid-cols-1 gap-4 mb-4">
              <div>
                <label class="block text-sm font-medium mb-2">Title</label>
                <input
                  v-model="option.title"
                  type="text"
                  class="w-full bg-gray-700 border border-gray-600 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
                  placeholder="Enter option title"
                />
              </div>
              <div>
                <label class="block text-sm font-medium mb-2"
                  >Description</label
                >
                <textarea
                  v-model="option.description"
                  rows="3"
                  class="w-full bg-gray-700 border border-gray-600 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
                  placeholder="Enter option description"
                ></textarea>
              </div>
            </div>
          </div>
        </div>

        <!-- Design Options Grid -->
        <div class="overflow-x-auto">
          <table class="w-full mb-4 border-collapse">
            <thead>
              <tr class="bg-gray-800">
                <th class="p-2 border border-gray-700">Criterion</th>
                <th
                  v-for="(option, index) in designOptions"
                  :key="index"
                  class="p-2 border border-gray-700"
                >
                  Option {{ String.fromCharCode(65 + index) }}
                  <div class="text-xs text-gray-400 mt-1">
                    {{ option.title || "Untitled" }}
                  </div>
                </th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="criterion in criteria" :key="criterion.name">
                <td class="p-2 border border-gray-700">{{ criterion.name }}</td>
                <td
                  v-for="(option, optionIndex) in designOptions"
                  :key="optionIndex"
                  class="p-2 border border-gray-700"
                >
                  <input
                    v-model.number="option.scores[criterion.name]"
                    type="number"
                    min="0"
                    max="10"
                    class="w-full bg-gray-800 border border-gray-700 rounded px-2 py-1 focus:outline-none focus:ring-2 focus:ring-blue-500"
                  />
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

      <!-- Generated Prompt -->
      <div class="mb-6">
        <div class="flex justify-between items-center mb-2">
          <label class="text-sm font-medium">Generated GPT Prompt</label>
          <div class="flex gap-2">
            <button
              @click="copyToClipboard"
              class="px-3 py-1 bg-green-600 text-sm rounded-lg hover:bg-green-700 transition"
            >
              Copy to Clipboard
            </button>
            <button
              @click="exportPrompt"
              class="px-3 py-1 bg-purple-600 text-sm rounded-lg hover:bg-purple-700 transition"
            >
              Export to File
            </button>
          </div>
        </div>
        <pre
          ref="promptDisplay"
          class="w-full bg-gray-800 border border-gray-700 rounded-lg p-4 whitespace-pre-wrap break-words"
          >{{ generatedPrompt }}</pre
        >
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from "vue";

// Form state
const title = ref("");
const problemStatement = ref("");
const constraints = ref([""]);
const criteria = ref([{ name: "", weight: 5 }]);
const designOptions = ref([
  { title: "", description: "", scores: {} },
  { title: "", description: "", scores: {} },
]);

// Helper methods
const addConstraint = () => {
  constraints.value.push("");
};

const removeConstraint = (index) => {
  constraints.value.splice(index, 1);
};

const addCriterion = () => {
  criteria.value.push({ name: "", weight: 5 });
};

const removeCriterion = (index) => {
  criteria.value.splice(index, 1);
};

const addDesignOption = () => {
  designOptions.value.push({ title: "", description: "", scores: {} });
};

const removeDesignOption = (index) => {
  designOptions.value.splice(index, 1);
};

// Computed property for generating the prompt
const generatedPrompt = computed(() => {
  const prompt = `Please generate a LaTeX report with the following structure and content:

TITLE: ${title.value}

PROBLEM STATEMENT:
${problemStatement.value}

CONSTRAINTS:
${constraints.value
  .filter((c) => c.trim())
  .map((c) => `- ${c}`)
  .join("\n")}

CRITERIA AND WEIGHTS:
${criteria.value
  .filter((c) => c.name.trim())
  .map((c) => `- ${c.name} (Weight: ${c.weight}/10)`)
  .join("\n")}

DESIGN OPTIONS EVALUATION:
${designOptions.value
  .map((option, index) => {
    const letter = String.fromCharCode(65 + index);
    return `Option ${letter}: ${option.title || "Untitled"}
Description: ${option.description || "No description provided"}
${criteria.value
  .filter((c) => c.name.trim())
  .map((c) => `- ${c.name}: ${option.scores[c.name] || 0}/10`)
  .join("\n")}`;
  })
  .join("\n\n")}

Please generate a complete LaTeX document with the following sections:
1. Title Page
2. Problem Statement
3. Background Research (with placeholder content)
4. Constraints
5. Criteria
6. Concept Evaluation Table (including weights and scores)
7. Final Design Description (based on the highest scoring option)

Requirements:
- Use proper LaTeX formatting and structure
- Include necessary packages and preamble
- Create professional tables for the evaluation matrix
- Format all sections with appropriate headings
- Include proper cross-referencing
- Add placeholder text where appropriate
- Ensure the document compiles without errors

Please provide the complete LaTeX code that I can directly compile.`;

  return prompt;
});

// UI helper methods
const promptDisplay = ref(null);

const copyToClipboard = async () => {
  try {
    await navigator.clipboard.writeText(generatedPrompt.value);
  } catch (err) {
    console.error("Failed to copy text:", err);
  }
};

const exportPrompt = () => {
  const blob = new Blob([generatedPrompt.value], { type: "text/plain" });
  const url = window.URL.createObjectURL(blob);
  const a = document.createElement("a");
  a.href = url;
  a.download = "latex-report-prompt.txt";
  a.click();
  window.URL.revokeObjectURL(url);
};
</script>
