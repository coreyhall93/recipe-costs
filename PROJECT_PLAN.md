# Recipe Cost Dashboard - Project Export for Claude Code

## Project Overview
Building an interactive web-based recipe cost tracking dashboard that:
- Tracks ingredient prices from grocery purchases
- Calculates recipe costs automatically
- Enables meal planning with daily/weekly/monthly cost projections
- Syncs data across devices via GitHub
- Hosted as a single HTML file on GitHub Pages

## User Context
- User: Corey (Happiness Engineer at Automattic)
- Location: Florence, SC
- Primary shopper: Walmart
- Use case: Compare homemade meal costs vs eating out
- Devices: Work laptop, personal laptop, iPad, iPhone (needs cross-device sync)

## Design Specification - FINAL

### Design Framework
**Tailwind CSS** with Apple-inspired aesthetic

### Color Palette
- **Primary accent**: Sky 600 (`oklch(58.8% 0.158 241.966)`)
- **Neutrals**: Stone palette (50, 100, 500, 600, 900)
- **Background**: `stone-50`
- **Cards**: `white`
- **Text primary**: `stone-900`
- **Text secondary**: `stone-500`, `stone-600`

### Typography
- **Font**: Inter (weights 300, 400, 500, 600, 700, 800, 900)
- **Main headings**: `font-black` (900 weight), `tracking-tight`
- **Section headings**: `text-2xl font-black`
- **Card titles**: `text-sm font-semibold text-sky-600 uppercase`
- **Large numbers**: `text-4xl font-black` for recipe costs and stats
- **Page title**: `text-5xl font-black`
- **Body text**: `text-sm` for most content
- **Cost per unit**: `font-mono` for precision

### Visual Elements
- **Card radius**: `rounded-2xl` (16px)
- **Card padding**: `p-8`
- **Card shadows**: `shadow-sm` with hover lift effect
- **Spacing**: Generous whitespace, `mb-16` between major sections
- **Borders**: `border-stone-100` for subtle dividers
- **Transitions**: `transition-all duration-200` for smooth interactions
- **Hover effects**: 
  - Cards: `transform: translateY(-2px)` + enhanced shadow
  - Table rows: `hover:bg-stone-50`
  - Buttons: Darken on hover

### Layout
- **Max width**: `max-w-[1280px]` centered
- **Grid**: `grid-cols-1 lg:grid-cols-3` for recipe cards
- **Grid**: `grid-cols-1 md:grid-cols-3` for stats
- **Responsive**: Mobile-first, stacks on small screens

## Current Implementation Status

### Completed (v1)
- Basic static dashboard showing:
  - English Muffins recipe cost ($2.13/batch, $0.27 each)
  - Pullman Sandwich Loaf cost ($2.01/loaf)
  - White Lily Biscuits cost ($0.89/batch, $0.07 each)
- Ingredient price database with 17 items
- Automatic cost calculations
- Recipe ingredient breakdowns
- Final design approved: Apple-inspired Tailwind with Sky 600 + Stone palette

## Required Features for v2

### 1. Ingredient Database (Editable)
**Table structure:**
```javascript
{
  id: "bread_flour",
  name: "Bread Flour",
  packageSize: "5 lbs (2268g)",
  price: 5.58,
  unit: "g", // or "oz", "each", "slice"
  costPerUnit: 0.0025 // auto-calculated
}
```

**UI Requirements:**
- Add/edit/delete ingredients
- Auto-calculate cost per unit
- Inline editing in table
- Validation (prevent negative prices, empty names)

### 2. Recipe Builder
**Recipe structure:**
```javascript
{
  id: "english_muffins",
  name: "English Muffins",
  yield: 8,
  ingredients: [
    { ingredientId: "bread_flour", amount: 430, unit: "g" },
    { ingredientId: "yeast", amount: 5, unit: "g" },
    // ...
  ],
  totalCost: 2.13, // auto-calculated
  costPerUnit: 0.27 // auto-calculated
}
```

**UI Requirements:**
- Create new recipes
- Select ingredients from dropdown (populated from database)
- Enter amounts and units
- Specify yield (how many items the recipe makes)
- See live cost calculation as ingredients are added
- Save recipe (becomes available as an ingredient itself)
- Edit/delete existing recipes

### 3. Meal Builder
**Meal structure:**
```javascript
{
  id: "bacon_egg_cheese_muffin",
  name: "Bacon Egg Cheese English Muffin Sandwich",
  components: [
    { type: "recipe", id: "english_muffins", quantity: 1 },
    { type: "ingredient", id: "bacon", quantity: 2, unit: "slice" },
    { type: "ingredient", id: "eggs", quantity: 1, unit: "each" },
    { type: "ingredient", id: "cheese", quantity: 1, unit: "slice" }
  ],
  totalCost: 0.89 // auto-calculated
}
```

**UI Requirements:**
- Create meals combining recipes + raw ingredients
- Dropdown shows both recipes and base ingredients
- Live cost calculation
- Save/edit/delete meals

### 4. Consumption Calculator
**UI Requirements:**
- Select a meal from dropdown
- Enter "servings per day"
- Display:
  - Cost per day
  - Cost per week (× 7)
  - Cost per month (× 30)
- Multiple meals can be tracked simultaneously
- Summary section showing total daily/weekly/monthly costs across all tracked meals

### 5. GitHub Data Sync
**Technical implementation:**
- Store all data in `data.json` file in same repo as HTML
- Use GitHub API to read/write the file
- User provides Personal Access Token (PAT) via settings panel
- Settings stored in localStorage (token, repo owner, repo name)
- Auto-load data.json on page load
- "Save to GitHub" button commits updated data.json
- Show sync status (last synced timestamp, sync in progress, errors)

**data.json structure:**
```json
{
  "ingredients": [ /* array of ingredient objects */ ],
  "recipes": [ /* array of recipe objects */ ],
  "meals": [ /* array of meal objects */ ],
  "consumption": [ /* array of consumption tracking objects */ ],
  "lastUpdated": "2025-02-12T22:30:00Z"
}
```

**GitHub API calls needed:**
- GET to fetch current data.json (with SHA for updates)
- PUT to update data.json (requires SHA from GET)
- Authentication via PAT in headers

## Current Ingredient Database

### Base Ingredients (as of 2025-02-12)
```javascript
{
  breadFlour: { name: 'Bread Flour', size: '5 lbs (2268g)', price: 5.58, perGram: 0.0025 },
  apFlour: { name: 'King Arthur AP Flour', size: '5 lbs (2268g)', price: 5.24, perGram: 0.0023 },
  selfRisingFlour: { name: 'White Lily Self-Rising', size: '5 lbs (2268g)', price: 4.44, perGram: 0.0020 },
  yeast: { name: 'Active Dry Yeast', size: '4 oz (113g)', price: 5.48, perGram: 0.0485 },
  butter: { name: 'Unsalted Butter', size: '4 lbs (1814g)', price: 8.24, perGram: 0.0045 },
  milk: { name: '2% Milk', size: '1 gallon (3785g)', price: 2.42, perGram: 0.0006 },
  honey: { name: 'Honey', size: '12 oz (340g)', price: 3.74, perGram: 0.0110 },
  sugar: { name: 'Granulated Sugar', size: '4 lbs (1814g)', price: 2.97, perGram: 0.0016 },
  brownSugar: { name: 'Light Brown Sugar', size: '32 oz (907g)', price: 1.94, perGram: 0.0021 },
  salt: { name: 'Morton Kosher Salt', size: '48 oz (1361g)', price: 2.97, perGram: 0.0022 },
  oliveOil: { name: 'Extra Virgin Olive Oil', size: '25.5 oz (724g)', price: 9.12, perGram: 0.0126 },
  eggs: { name: 'Large White Eggs', size: '18 count', price: 2.92, perUnit: 0.1622 },
  bacon: { name: 'Hickory Smoked Bacon', size: '12 oz', price: 3.77, perOz: 0.314 },
  sausage: { name: 'Breakfast Sausage Patties', size: '12 oz', price: 2.42, perOz: 0.202 },
  bakingPowder: { name: 'Baking Powder', size: '8.1 oz (230g)', price: 1.98, perGram: 0.0086 },
  bakingSoda: { name: 'Baking Soda', size: '8 oz (227g)', price: 0.87, perGram: 0.0038 },
  cheese: { name: 'American Cheese Singles', size: '24 slices', price: 2.48, perSlice: 0.1033 },
  vinegar: { name: 'White Vinegar', size: '16 oz (473g)', price: 1.50, perGram: 0.0032 }
}
```

## Current Recipes

### English Muffins
**Yield:** 8 muffins
**Ingredients:**
- 430g bread flour → $1.08
- 5g active dry yeast → $0.24
- 35g butter → $0.16
- 75g 2% milk → $0.05
- 20g honey → $0.22
- 12g sugar → $0.02
- 12g salt → $0.03
- 28g olive oil → $0.33
**Total:** $2.13 per batch ($0.27 per muffin)

### Pullman Sandwich Loaf
**Yield:** ~16 slices (adjustable)
**Ingredients:**
- 500g bread flour → $1.25
- 9g active dry yeast → $0.43
- 55g butter → $0.25
- 30g 2% milk → $0.02
- 25g sugar → $0.04
- 10g salt → $0.02
**Total:** $2.01 per loaf

### White Lily Biscuits
**Yield:** 12 biscuits
**Ingredients:**
- 240g self-rising flour → $0.48
- 57g butter → $0.26
- 177g milk (for buttermilk) → $0.11
- 5g vinegar → $0.00
- 28g melted butter → $0.13
**Total:** $0.89 per batch ($0.07 per biscuit)

## User Experience Flow

1. **First time setup:**
   - User opens dashboard
   - Enters GitHub settings (PAT, repo owner, repo name)
   - System attempts to load data.json
   - If not found, initializes with current ingredient/recipe data
   - User can start editing immediately

2. **Daily usage:**
   - Page loads data.json automatically
   - User updates prices when shopping
   - User adds new recipes as they try them
   - User builds meals and tracks consumption
   - Clicks "Save to GitHub" when done
   - All devices sync to same data.json

3. **Cross-device workflow:**
   - User updates price on work laptop
   - Commits to GitHub
   - Opens dashboard on iPad later
   - iPad loads updated data.json automatically
   - No manual export/import needed

## Technical Constraints
- Single HTML file (can include CSS/JS inline or via CDN)
- No backend server required
- Must work on GitHub Pages
- Cross-browser compatible (Chrome, Safari, Firefox)
- Mobile-responsive design

## Build Instructions for Claude Code

### Phase 1: Foundation
1. Start with `example-apple.html` as design template
2. Set up data management system:
   - localStorage for GitHub settings (PAT, repo, owner)
   - In-memory state for all app data
   - GitHub API integration functions
3. Create initial data.json structure with seed data (current ingredients + recipes)

### Phase 2: Core Features
Build in this order:

**Step 1: Ingredient Management**
- Editable table with inline editing
- Add new ingredient form
- Delete ingredient with confirmation
- Auto-calculate cost per unit
- Validation (no negative prices, required fields)

**Step 2: Recipe Builder**
- Recipe creation form
- Ingredient selector (dropdown from ingredient database)
- Amount/unit inputs
- Yield input
- Live cost calculation display
- Save recipe (adds to recipes array AND makes available as ingredient)
- Recipe list view
- Edit/delete recipes

**Step 3: Meal Builder**
- Meal creation form
- Component selector (dropdown shows both recipes AND ingredients)
- Quantity inputs
- Live cost calculation
- Save/edit/delete meals
- Meal list view

**Step 4: Consumption Calculator**
- Meal selector dropdown
- Servings per day input
- Display cards showing:
  - Daily cost (servings × meal cost)
  - Weekly cost (daily × 7)
  - Monthly cost (daily × 30)
- Allow multiple meals to be tracked
- Total summary across all tracked meals

**Step 5: GitHub Sync**
- Settings panel for PAT, repo owner, repo name
- Load data.json on page load
- Save to GitHub button
- Sync status indicator
- Error handling and user feedback

### Phase 3: Polish
- Loading states
- Error messages
- Form validation feedback
- Success confirmations
- Mobile responsive testing
- Cross-browser testing

## File Structure
```
recipe-costs/
├── index.html          (main dashboard - single file with all JS/CSS)
├── data.json           (data storage)
└── README.md           (setup instructions)
```

## GitHub API Example Code

```javascript
// Load data from GitHub
async function loadDataFromGitHub(owner, repo, token) {
  const url = `https://api.github.com/repos/${owner}/${repo}/contents/data.json`;
  const response = await fetch(url, {
    headers: {
      'Authorization': `Bearer ${token}`,
      'Accept': 'application/vnd.github.v3+json'
    }
  });
  
  if (response.ok) {
    const data = await response.json();
    const content = JSON.parse(atob(data.content));
    return { content, sha: data.sha };
  }
  return null;
}

// Save data to GitHub
async function saveDataToGitHub(owner, repo, token, data, sha) {
  const url = `https://api.github.com/repos/${owner}/${repo}/contents/data.json`;
  const content = btoa(JSON.stringify(data, null, 2));
  
  const response = await fetch(url, {
    method: 'PUT',
    headers: {
      'Authorization': `Bearer ${token}`,
      'Accept': 'application/vnd.github.v3+json',
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      message: 'Update recipe costs data',
      content: content,
      sha: sha
    })
  });
  
  return response.ok;
}
```

## Future Enhancements (Out of Scope for v2)
- Paste-and-parse ingredient lists
- Price history tracking over time
- Multi-store price comparison
- Recipe scaling (double/halve recipes)
- Nutrition information integration
- Shopping list generation
- Export to PDF/Excel

## Design Preferences (User's Style)
From user preferences:
- Human, editorial voice
- Avoid AI-sounding language
- Use headers and lists less
- Be empathetic without being performative
- State points directly and affirmatively
- Avoid casual prefaces, filler, conversational padding
- Prefer clear, declarative sentences with uneven, natural rhythm
- Prioritize clarity, precision, and integrity

Apply to UI:
- Clean, direct labels
- No unnecessary helper text or tooltips unless truly needed
- Straightforward interactions
- Professional but not sterile

## Critical Success Factors
1. **Data persistence**: GitHub sync must work reliably across devices
2. **Recipes as ingredients**: Once a recipe is created, it must be usable in meals
3. **Live calculations**: All costs update immediately as user makes changes
4. **Mobile-friendly**: Must work well on iPhone/iPad
5. **Single file deployment**: Everything in one HTML file for easy GitHub Pages hosting

## Testing Checklist
- [ ] Create ingredient
- [ ] Edit ingredient price (cost per unit updates automatically)
- [ ] Delete ingredient
- [ ] Create recipe using ingredients
- [ ] Recipe cost calculates correctly
- [ ] Recipe appears in meal builder dropdown
- [ ] Create meal using recipe + ingredients
- [ ] Meal cost calculates correctly
- [ ] Track multiple meals in consumption calculator
- [ ] Daily/weekly/monthly costs calculate correctly
- [ ] Save to GitHub (data.json updates)
- [ ] Load from GitHub on fresh page load
- [ ] Test on mobile device
- [ ] Test cross-device sync (update on laptop, view on iPad)

## Questions for User (Already Answered)
- ✅ Which design framework? **Tailwind with Apple aesthetic**
- ✅ Color scheme? **Sky 600 + Stone palette**
- ✅ Font weights? **Black (900) for headings**
- ✅ Recipe card sizing? **text-4xl for costs**
