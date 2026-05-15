import streamlit as st

class FormulaAgent:
    def __init__(self):
        self.database = {
            "designer wellness strawberry banana protein smoothie": {
                "serving_size_g": 120,
                "nutrition": {
                    "calories": 120,
                    "fat_g": 4,
                    "carbs_g": 7,
                    "protein_g": 12
                },
                "ingredients": [
                    "Water",
                    "Whey Protein Isolate",
                    "Apple Juice Concentrate",
                    "Strawberry Puree",
                    "High Oleic Sunflower Oil",
                    "Banana Puree",
                    "Monk Fruit Concentrate",
                    "Natural Flavors",
                    "MCT Oil",
                    "Citric Acid",
                    "Apple Pectin",
                    "Sunflower Lecithin",
                    "Sea Salt"
                ]
            }
        }

    def run(self, name):
        key = name.lower().strip()
        if key not in self.database:
            return None

        product = self.database[key]

        scale = 100 / product["serving_size_g"]
        nutrition = {k: round(v * scale, 2) for k, v in product["nutrition"].items()}

        protein = nutrition["protein_g"]
        fat = nutrition["fat_g"]
        carbs = nutrition["carbs_g"]

        formula = {
            "Water": 70,
            "Whey Protein Isolate": protein * 1.1,
            "Fruit & Juice Blend": carbs * 2,
            "Oils (Sunflower + MCT)": fat,
            "Stabilizers + Flavor + Minerals": 5
        }

        total = sum(formula.values())
        formula = {k: round(v * 100 / total, 2) for k, v in formula.items()}

        return nutrition, product["ingredients"], formula


st.title("🥤 Product → Formula Agent")

agent = FormulaAgent()

product = st.text_input("Enter product name:")

if st.button("Generate Formula"):

    result = agent.run(product)

    if result is None:
        st.error("Product not found (add it in app.py)")
    else:
        nutrition, ingredients, formula = result

        st.subheader("Nutrition (per 100g)")
        st.write(nutrition)

        st.subheader("Ingredients")
        st.write(ingredients)

        st.subheader("100g Formula")
        st.write(formula)
