# CrewAI pantry-to-plate recipe planner

This project uses CrewAI to create a team of AI agents that help you figure out what to cook using **only the ingredients you already have**. Provide a list of your available ingredients, any dietary restrictions, and desired meal types, and the crew will suggest recipe ideas, generate basic instructions using only those items (plus assumed staples), and verify that no extra ingredients were needed.

This example is designed to run in a Google Colab notebook.

## Features

*   **Ingredient analysis:** Parses and understands your list of available ingredients.
*   **Recipe ideation:** Brainstorms meal ideas that utilize your core ingredients and respect dietary constraints.
*   **Constrained recipe generation:** Generates plausible ingredient lists (using available items) and basic cooking instructions for suggested recipes, adding **no external ingredients** beyond basic staples (salt, pepper, oil).
*   **Constraint verification:** Checks if the generated recipes strictly adhere to the available ingredient list.
*   **Structured output:** Presents the generated recipes and the verification result in a clear, usable format.

## Requirements

*   Python 3.9+
*   Google Colab environment (recommended) or local Python setup.
*   **API Keys:**
    *   **OpenAI API key:** For the language model (GPT-4 Turbo recommended for better recipe generation/understanding). Requires a funded account or active credits from [platform.openai.com](https://platform.openai.com/).
    *   **Serper API key:** *(Note: While included in setup, this version relies less on web search for recipes and more on generation, but the key is still configured in the code).* Get one from [serper.dev](https://serper.dev/) if needed for other potential modifications.

## Setup (Google Colab)

1.  **Get the notebook:** Clone this repository or download the `.ipynb` file.
2.  **Open in Colab:** Upload and open the notebook in Google Colab ([colab.research.google.com](https://colab.research.google.com/)).
3.  **Install libraries:** Run the first cell (`# @title 1. Install...`) to install dependencies.
4.  **Configure API keys in Colab secrets:**
    *   Open Colab secrets (Key icon left sidebar).
    *   Enable "Notebook access".
    *   Add two secrets (though Serper is less critical now):
        *   Name: `OPENAI_API_KEY` | Value: Your OpenAI key (`sk-...`)
        *   Name: `SERPER_API_KEY` | Value: Your Serper key
    *   Ensure the toggle switch is **ON** for both secrets.
5.  **Run Cell 2:** Execute the second cell (`# @title 2. Import Modules...`). Check the output to confirm the OpenAI API key was found.

## How to use

1.  **Define your inventory & preferences (Cell 3):**
    *   Go to Cell 3 (`# @title 3. Define pantry inventory...`).
    *   **Modify `available_ingredients`:** List all the ingredients you have. Be reasonably specific.
    *   **Modify `dietary_restrictions`:** Specify constraints or set to "None".
    *   **Modify `desired_meal_types`:** Indicate the type of meal.
2.  **Select LLM (optional - Cell 4):**
    *   Ensure an appropriate model (GPT-4 Turbo recommended) is selected in Cell 4.
3.  **Run cells sequentially:** Execute the remaining cells (4 through 8) in order.
    *   Cell 5 defines the agents (using the "Strict Inventory" versions).
    *   Cell 6 defines the tasks (using the "Strict Inventory" versions).
    *   **Cell 7** creates the `Crew` and runs `kickoff()`. This involves AI generation and analysis based *only* on your inputs.
    *   **Cell 8** displays the final generated recipes and the result of the constraint verification.

## Workflow overview

1.  **Ingredient analyzer (Task 1):** Understands what ingredients are listed as available.
2.  **Recipe ideator (Task 2):** Brainstorms meal ideas using only those ingredients.
3.  **Recipe detail generator (Task 3):** *Generates* instructions and ingredient lists using *only* the available items (+ staples).
4.  **Recipe constraint verifier (Task 4):** *Checks* if the previous step correctly used only the allowed ingredients.
5.  **Meal plan presenter (Task 5):** Formats the generated recipes and the verification result clearly.

## Customization

*   **Input details:** Provide more or less detail in the ingredient list or preferences in Cell 3.
*   **Number of recipes:** Change the number requested in the `RecipeIdeator` goal/task description (Cell 5/6).
*   **Assumed staples:** The assumption of salt/pepper/oil is currently hardcoded in the prompts (Cell 5/6). Modify agent goals/task descriptions if you want to change this assumption.
*   **Output format:** Adjust the `MealPlanPresenter` task description for different formatting.

## Limitations & disclaimer

*   **Recipe simplicity/unconventionality:** Forcing recipes to use only limited ingredients might result in very basic, simple, or potentially unconventional meal ideas compared to standard recipes.
*   **Generated recipe reliability:** Since recipes are generated based on constraints rather than fetched from known sources, their quality, taste balance, cooking times, and ingredient quantity estimations may require significant user judgment and adjustment during cooking. Use common sense.
*   **Dietary needs:** While the crew attempts to follow restrictions, **always double-check ingredients** yourself, especially for allergies or strict dietary requirements. This tool is not a substitute for careful personal checking.
*   **Ingredient interpretation:** The AI might misunderstand obscure ingredients or quantities listed in the initial input. Clear input helps.
*   **API costs:** Uses OpenAI API credits.
