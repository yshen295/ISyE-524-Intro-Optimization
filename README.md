# A Strategist's Guide to Campus Dining: An Optimization Project

This project, developed for ISyE 524: Introduction to Optimization, tackles a common student dilemma: "Where should I eat lunch this week?" We formulate this problem as a Mixed-Integer Linear Program (MILP) to generate an optimal 5-day dining plan across six campus restaurants.

Our paper submission (top course project for summer 2025) is found at ['main.pdf'](/workspaces/ISyE-524-Intro-Optimization/main.pdf).

The primary source code can be found in the Jupyter Notebook: [`Final_pj_source_code.ipynb`](/workspaces/ISyE-524-Intro-Optimization/Final_pj_source_code.ipynb).

## The Optimization Model

The model's primary objective is to minimize a weighted combination of total travel/wait time and meal costs. The preferences for time versus money can be adjusted for each day of the week, reflecting changing priorities.

### Key Features & Constraints

The model incorporates several real-world complexities to find a practical and satisfying dining schedule:

*   **Daily Choice**: You must eat at exactly one restaurant each day.
*   **Visit Frequency**: Any single restaurant can be visited at most twice per week to ensure variety.
*   **Dynamic Enjoyment**: An "enjoyment index" is modeled to favor variety. This index decreases with repeated visits to the same restaurant and is implemented using Special Ordered Sets of Type 2 (SOS2).
*   **Promotions & Deals**: The model accounts for restaurant-specific discounts, such as "Taco Tuesday," to find the most economical options.
*   **Weekly Nutrition**: The final plan must satisfy weekly nutritional targets, including lower and upper bounds for calories, fat, protein, and carbohydrates.

### Methodology

The model is built in Julia using the `JuMP` modeling language and solved with the Gurobi optimizer.

A key feature of the solution approach is the use of an iterative cutting-plane method to handle nutritional constraints. Instead of loading all nutritional information into the main model, we first solve for the best plan based on time, cost, and enjoyment. We then check if this plan meets the weekly nutritional goals. If it doesn't, we add a "no-good cut" to the model—a constraint that excludes that specific infeasible solution—and re-solve. This process repeats until a plan is found that satisfies all constraints.

