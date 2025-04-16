# Challenges of Staking

Staking is often seen as a simple way for token holders to earn passive income while supporting the security of the blockchain networks. However, staking involves navigating a range of risks and variables associated with validator selection, variable staking yields, liquid staking risks, and unbonding periods. Choosing an underperforming validator might lead to reduced rewards and even penalties through slashing.

# Existing Research in Staking Optimization

The most prominent Machine Learning research pieces on Cryptocurrency focus on predicting the future price (Amirzadeh et al., 2022; D’Amato et al., 2022; Derbentsev et al., 2021; Hamayel and Owda, 2021; Kyriazis et al., 2020; Mudassir et al., 2020). This is assumed to be due to the ease at which one can access cryptocurrency pricing data. The only paper that sought to forecast staking rewards was that of Gupta et al. (2024). We referenced his research to test whether it can provide us with accurate forecasts.

# Feature Selection

The model by Gupta et al. (2024) utilized staking coin prices, time-series of staking rewards, and staking coin trends to predict staking rewards yield for the next n-days. My model included the coin prices and staking reward yield data; however, rather than focusing on coin-specific trends, I decided to incorporate a fear and greed index to analyze the impact of overall market sentiment.

# Model Pipeline

For my initial forecasts, I replicated the models utilized by Gupta et al. (2024), namely the multi-linear regression (MLR) model and the moving-window average (MWA) model. However, my models differ from the paper in a few key areas:
- The paper used data from 2021 to 2022, whereas my model used data from 2022 to 2025, which is after the merger when Ethereum transitioned to PoS from PoW model.
-	The paper used 3 months of data to predict staking reward yield for the next 30 days, however, my model used the traditional 80 – 20 train-test split to predict staking reward yield for the next 14 days.
-	The paper evaluated its model based on RMSE/Mean metric, which normalized the error relative to the size of the data (implying lower values), whereas my model was evaluated based on the raw R2 and RMSE metrics. 

# Model Results
For ETH, the MWA model had an average RMSE of 0.085, and MLR had 0.172. For SOL, MWA had 0.218, and MLR had 0.416 (Table 1). The R2 metrics showed ETH with 0.821 under MLR and 0.907 under MWA, indicating reasonable predictive capabilities. SOL had 0.155 under MLR and 0.685 under MWA, showing poorer predictive capabilities (Table 2). Thus, ETH predictions are more stable and reliable than SOL.

This approach might have failed due to the simplicity of the MWA and MLR models. MWA uses fixed window sizes, and MLR assumes linear relationships, which don't adapt well to the non-linear dynamics of the volatile cryptocurrency market. To address these shortcomings, we developed a new model using Neural Network techniques for predicting staking reward yields.

# Novel Approach – Neural Network

I explored using a basic Neural Network (NN) model for this task. NNs come in various forms for different problems. For instance, Convolutional Neural Networks (CNN) excel at image and video classification, Long Short-Term Memory Networks (LSTM) are ideal for time series forecasting and sequential tasks, and Transformer Networks are best for Natural Language Processing (NLP) tasks like translation and text summarization. Despite their differences, all these models share the core fundamentals of Neural Networks.

# Model Analysis

The NN model, which predicts the staking reward yield of ETH and SOL, achieved high accuracy, as well as in my evaluation metrics - high R2 and low RMSE. The model performed exceptionally well as it excels at capturing the complexity and non-linearity between all input features, whereas the traditional MWA and MLR methods used in prior research are linear models and could not capture non-linear relationships. This is possible due to the existence of an Activation Function in the NN. Inspired by the work of Agarwal et al. 2021, I wrote a custom Exponentially Centered Unit (ExU) function that behaves linearly for non-negative inputs and applies a scaled hyperbolic tangent function for negative inputs, providing a smooth transition around zero, allowing us to capture the said non-linear relationship between inputs.
my high R² values of 0.987 and 0.985 in my prediction model for ETH and SOL, respectively, suggest that my model could explain almost all the variation in the staking reward yield. This reflects the robustness of NN as well as the effectiveness of my chosen inputs. Additionally, the low RMSE of approximately 0.0002 across all days indicates high precision, where the predicted points are very close to the actual points. These results highlight the model’s ability to handle trends and fluctuations across the period, especially in the volatile cryptocurrency environment. 
Despite these results, there are some areas where improvements can be made. At present, the model’s performance has only been assessed based on the limited ETH and SOL datasets, with the model trained on more than 1 year of data and tested on more than 3 months of data. This should and will be improved to provide the model with varying market conditions and duration of train test, such that the model can provide a more adaptive performance. With my current limited data, the model does struggle slightly with extreme anomalies or volatility, where the predicted crest fails to reach the actual crest, as evidenced by the graph in the appendix. Furthermore, the model also struggles with extended duration forecasts, where the model’s predictions begin to deviate further from the actual trend the longer the forecasts are made. This deviation is a sign that the model lacked sufficient data to predict long-term forecasts.

# Discussion

While I have established a basic model for optimizing staking reward yield using machine learning, I intend to explore further and improve the model, developing it into a comprehensive model capable of providing public services. 
In this section, I will first discuss the unexplored features that are potentially useful for my feature engineering, followed by future business opportunities.

# Feature Exploration and Improvements

The unexplored features are shown below:
| Category           | Features                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------|
| **Epoch-specific** | Changes in staking yields, inflation rate, and total active stake across epochs.           |
| **Validator-specific** | Validator performance, Validator uptime, Validator slashing rate, Volume of delegators, and Distribution of staked token per validator. |
| **On-chain specific** | Percentage of total staked supply and Network participation rate in staking.            |

These unexplored features are considered for potentially improving the predictive capabilities of my model, for example, the epoch-specific variables would improve the model by capturing the variation of patterns across epochs.

While my current approach was with a basic NN, more complex and sophisticated models like Graph Neural Networks (GNN) can be utilized with the availability of more data mentioned above. A model like GNN visualizes the inputs in a non-Euclidean graph space to capture the spatial and temporal features to understand the relationship between all the input features. However, it must be noted that with approaches like these the computational time and costs increase.
