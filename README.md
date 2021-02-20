# TP_test
Repository for the technical assesment for Teleperformance.

## Exploration and preprocessing

We first check the relationship between the objective variable and its interactions with the others.

For that, we first make a tabular revision of the independent variables against the objective in order to make a preselection of those that might give us good information for the model.	

From this first stage, I conclude that the inclusion of the categorical variables 'REGIONAL', 'DEPARTAMENTO', 'TECNOL', 'GERENCIA', 'CANAL_HOMOLOGADO_MILLICON' and 'fallo' would not be helpful, because:

- There are too many categories, which may lead to overfitting problems.
- The proportions of unpayers vs payers did not vary significantly from the global.
- The proportions did vary but for very small categories.

Now, for further variable selection I will create a shallow decision tree classifier to see which variables appear on the upper levels. Decision trees work by establishing a frontier in the value of a variable which best separate the objective variables, thus, if a variable appears on the beginning of the splitting process, chances are it is important for the problem at hand. Similarly, if a variable does not show, it is likely that the information it introduces is very little and that it can be safely discarded.

After doing this with a tree of a maximum depth of five, for visualisation purposes, we can notice that the only variables that appear are  'estrato','productos', 'portafolio' and 'antiguedad_meses'; towards the bottom we start seeing sometimes 'tipo_fuerza_venta' and other binary variables like 'asesoria_servicios' or 'no_serv_tecnicos'. The first five will be included in the model. As for the other binary variables, I will include them since they have information about the interaction between the customer and the company, which the latter can monitor more efficiently if needed.

## Logistic regression

The model I will choose is a logistic regression. Since we are looking for the probability of belonging to a positive class, this method seems the most natural to use, also, we want to be able to know which variables have greater impact on said probability, which is easier do with a logistic regression compared to a neural network. 