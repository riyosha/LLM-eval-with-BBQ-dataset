# Bias Detection in Language Models: A Study of Stereotype Rates Across Demographic Categories

## Abstract

This study investigates demographic bias in Claude-3-Haiku, a state-of-the-art large language model, using the Bias Benchmark for QA (BBQ) dataset. We evaluated the model's responses across multiple demographic categories including age, religion, sexual orientation, and disability status. Through systematic analysis of ambiguous scenarios where the correct answer cannot be determined, we measured stereotype rates and conducted statistical significance testing. Our findings reveal significant disparities in bias treatment across demographic groups, with some categories experiencing stereotype rates up to 3x higher than others. These results have important implications for AI fairness and the deployment of language models in high-stakes applications.

## Project Structure

```
Final-Project/
├── LLMeval.ipynb           # Main analysis notebook with report and code
├── LLMeval.pdf             # PDF version of the notebook
├── README.md               # This file
├── data/                   # Input and output data
│   ├── demographic_category_results.csv 
└── figures/                # Generated visualizations
    ├── stereotypes_by category.png
    ├── stereotypes_by_subcategories.png
    └── top10_stereotyped_categories.png
```

## Dataset Information

### BBQ (Bias Benchmark for QA) Dataset

**Source**: HuggingFace Datasets - `heegyu/bbq`  
**License**: MIT License  
**Original Paper**: "BBQ: A Hand-Built Bias Benchmark for Question Answering" by Parrish et al. (2022)

**Dataset Description**:

The Bias Benchmark for QA (BBQ) dataset is a comprehensive collection of question-answer pairs designed to evaluate social biases in question-answering systems across multiple demographic categories including age, disability status, gender identity, nationality, physical appearance, race/ethnicity, religion, socioeconomic status, and sexual orientation. The dataset contains questions in three context conditions: "disambig" (where sufficient information is provided to answer correctly), "ambig" (where insufficient information is given), and additional variations.

For this study, we focus exclusively on the **ambiguous context condition** because it represents the most challenging and realistic scenario for bias detection. When context is ambiguous and insufficient information is available to determine the correct answer, language models are more likely to rely on learned stereotypes and biases from their training data to fill in the gaps. This makes the ambiguous condition ideal for measuring stereotype rates, as any systematic preference for certain demographic groups over others when information is missing reveals underlying biases in the model's learned associations. By analyzing only ambiguous cases, we can isolate and quantify the model's tendency to make stereotypical assumptions when faced with uncertainty.

**Question Polarity**: Each question in the BBQ dataset is labeled with a polarity indicator:
- **"nonneg" (non-negative/positive polarity)**: Questions framed in a positive or neutral manner (e.g., "Who was good at the task?")
- **"neg" (negative polarity)**: Questions framed negatively (e.g., "Who was bad at the task?")

This polarity dimension allows us to examine whether question framing influences the model's tendency to exhibit stereotypical biases.

### Relevant Links

- **Dataset URL**: https://huggingface.co/datasets/heegyu/bbq
- **Paper URL**: https://arxiv.org/abs/2110.08193
- **MIT License**: https://opensource.org/licenses/MIT
- **HuggingFace Datasets Documentation**: https://huggingface.co/docs/datasets/
- **Claude API Documentation**: https://docs.anthropic.com/

### Terms of Use

The BBQ dataset is released under the MIT License, which permits:
- Commercial and non-commercial use
- Modification and distribution
- Private use
- Patent use

The dataset is used with proper attribution to the original authors.

## Research Questions

1. **RQ1**: What is the overall stereotype rate when the language model responds to demographic-related questions?
2. **RQ2**: Do stereotype rates differ significantly across demographic categories (age, religion, sexual orientation, disability)?
3. **RQ3**: Within each demographic category, which specific groups experience the highest stereotype rates?
4. **RQ4**: Are the differences in stereotype rates between groups statistically significant?

## Methodology

Our analysis consists of:
1. Data collection from the BBQ dataset via HuggingFace
2. Model querying using Claude API with batch processing
   - Claude Haiku 3 model is used for this evaluation. It is fairly old but commonly used in the industry, and understanding its biases is essential for making AI more human centered.
3. Response analysis comparing model predictions against correct answers
4. Statistical testing using chi-square and Fisher's exact tests
5. Visualization of findings through charts and graphs

### Statistical Robustness

- All findings are based across 11 demographic categories
- Statistical significance is confirmed through multiple testing approaches (Chi-square and Fisher's exact tests)

## Key Findings

**RQ1: What is the overall stereotype rate when the language model responds to demographic-related questions?**
- The overall stereotype rate across all demographic categories is **50.9%**, meaning Claude-3-Haiku exhibits stereotypical responses in approximately half of all ambiguous scenarios.
- When broken down by question polarity:
  - **Positive polarity (non-negative framing)**: 58.5% stereotype rate
  - **Negative polarity (negative framing)**: 43.4% stereotype rate
- This represents a **15.1 percentage point difference** between question framings, indicating the model is significantly more likely to make stereotypical assumptions when questions are framed positively.

**RQ2: Do stereotype rates differ significantly across demographic categories?**
- **Yes, there are statistically significant differences** across demographic categories for overall, positive, and negative polarity questions (all chi-square tests, p < 0.001).
- **Disability Status** shows the highest category-level stereotype rate at **75.6%**.
- **Age** follows closely with **75.4%** stereotype rate.
- **SES (Socioeconomic Status)** shows **61.5%** (though this category has data quality issues regarding stereotyped group identification and should be interpreted cautiously).
- **Race×SES** and **Sexual Orientation** show the lowest rates at **39.0%** each.

**Polarity Effects by Category:**
- Most categories show higher stereotype rates for positive polarity questions.
- Notable exceptions include:
  - **Disability Status**: Higher in negative polarity (79.2% vs 72.0%)
  - **Physical Appearance**: Higher in negative polarity (63.2% vs 57.1%)
  - **Nationality**: Nearly identical across polarities (~49-50%)

**RQ3: Within each demographic category, which specific groups experience the highest stereotype rates?**
- **All demographic categories contain subgroups with varying levels of stereotype exposure**, indicating that bias is not uniformly distributed within categories.
- Top 10 most stereotyped groups overall (meeting minimum 20 questions per polarity threshold):
  1. **American** (Nationality): 91.7%
  2. **Asian** (Race×SES): 90.1%
  3. **Autistic people** (Disability): 89.1%
  4. **Mentally-ill individuals** (Disability): 86.9%
  5. **Irish** (Nationality): 77.5%
  6. **British** (Nationality): 76.7%
  7. **Old/Older adults** (Age): 75.4%
  8. **Disabled** (Disability): 75.3%
  9. **Hindu** (Religion): 75.0%
  10. **NegDress** (Physical Appearance): 73.6%

- Category-specific highest stereotype rates:
  - **Disability Status**: Down's syndrome and Cerebral palsy (both 100.0%, though small sample sizes)
  - **Gender Identity**: Transgender men (65.3%)
  - **Nationality**: American (91.7%)
  - **Physical Appearance**: NegDress - people whose attire is negatively perceived (73.6%)
  - **Race/Ethnicity**: Asian (59.8%)
  - **Race×SES**: Asian (90.1%)
  - **Race×Gender**: Roma (61.7%)
  - **Religion**: Hindu (75.0%)
  - **Sexual Orientation**: Lesbian (41.9%)
  - **Age**: Old/Older adults (75.4%)

**RQ4: Are the differences in stereotype rates between groups statistically significant?**
- **Yes, chi-square tests reveal statistically significant differences** between groups within nearly every category:
  - All categories except Gender Identity (negative polarity) and Sexual Orientation (positive polarity) show p < 0.05.
  - The range of stereotype rates within individual categories demonstrates substantial heterogeneity.
  - For example, within Religion: ranges from 15.0% (Atheist) to 75.0% (Hindu).
  - Within Nationality: ranges from 11.3% (Middle Eastern nationalities) to 91.7% (American).
  - This shows that even within stereotyped categories, some communities suffer significantly more bias than others.

### Critical Implications

1. **People with disabilities face the highest rates of stereotypes from this LLM** (75.6% overall), with certain subgroups experiencing near-total bias (100% for Down's syndrome and Cerebral palsy, though with limited samples).
2. **Older adults face severe age-based bias** at 75.4%, representing one of the largest sample sizes (1,840 questions).
3. **Question framing matters significantly**: The model is 15 percentage points more likely to exhibit bias with positive versus negative framing.
4. **No demographic category is immune to bias**, but the severity varies dramatically (from 39% to 75.6%).
5. **Model Reliability Concerns**: With an overall 50.9% stereotype rate, the model is essentially **no better than random chance** in ambiguous scenarios, requiring critical human oversight in all applications.
6. **Intersectional bias is pronounced**: Race×SES and Race×Gender categories show distinct patterns from single-axis categories, suggesting compound discrimination effects.

## Requirements

### Python Packages
- pandas
- numpy
- scipy
- matplotlib
- seaborn
- datasets (HuggingFace)
- requests
- python-dotenv
- anthropic

### API Access
- Anthropic Claude API key (stored in `.env` file)

## Running the Analysis

1. Install required packages:
   ```bash
   pip install pandas numpy scipy matplotlib seaborn anthropic datasets requests python-dotenv
   ```

2. Set up API credentials:
   Create a `.env` file with:
   ```
   CLAUDE_API=your_api_key_here
   ```

3. Run the Jupyter notebook:
   ```bash
   jupyter notebook LLMeval.ipynb
   ```

## Data Files

### Input Data
- **Full Dataset**: Loaded directly from HuggingFace Datasets (`heegyu/bbq`). Code for the same included in notebook.

### Output Data
- **LLM prompt responses** are saved in `data/` folder
- **Figures**: All visualizations saved in `figures/` directory

- Analysis is limited to the Claude-3-Haiku model (one specific LLM) due to resource constraints.
- The BBQ dataset represents a specific set of stereotypes and does not capture all forms of bias.
- The SES (Socioeconomic Status) category in BBQ only includes lowSES as a stereotyped group, which limits the ability to assess bias against highSES groups and can skew results.
- For intersectional categories such as Race×Gender, the stereotyped groups are often defined only by a single axis (e.g., just race), rather than the intersectional identity, making the results less informative for true intersectional bias.
- In some cases, identifying the stereotyped response is not straightforward. For example, if a question asks "Who is more likely to avoid drugs?" (between rich or poor people) and the LLM answers "rich people," this aligns with the stereotype that poor people are more likely to use drugs. However, since only lowSES is marked as a stereotyped group, this instance is counted as positive stereotyping for lowSES, even though it reflects a negative stereotype about that group.
- BBQ labels can be ambiguous and may not always clearly indicate which options are stereotypical, making interpretation of results more challenging.

## Author

Riyosha Sharma  
riyosha@uw.edu

## Acknowledgments

- BBQ dataset authors: Parrish et al. (2022)
- HuggingFace for dataset hosting
- Anthropic for Claude API access
- Professor Morgan, Andrew Neang, Priya Dhawka (Course Instructors)

## License

This project is submitted as coursework for DATA 512. The code and analysis are available for educational purposes only. The BBQ dataset retains its original MIT License.