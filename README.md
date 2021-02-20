# TP_test
Repository for the technical assesment for Teleperformance.

## Exploration and preprocessing

We first check the relationship between the objective variable and its interactions with the others.

For that, we first make a tabular revision of the independent variables against the objective in order to make a preselection of those that might give us good information for the model.	

From this first stage, I conclude that the inclusion of the categorical variables 'REGIONAL', 'DEPARTAMENTO', 'TECNOL', 'GERENCIA', 'CANAL_HOMOLOGADO_MILLICON' and 'fallo' would not be helpful, because:

- There are too many categories, which may lead to overfitting problems.
- The proportions of unpayers vs payers did not vary significantly from the global.
- The proportions did vary but for very small categories.

Now, for further variable selection I will create a shallow decision tree classifier to see which variables appear on the upper levels. Decision trees work by establishing a frontier in the value of a variable which best separate the objective variables, thus, if a variable appears on the beginning of the splitting process, chances are it is important for the problem at hand. Similarly, if a variable does not show, it is likely that the information it introduces is very little and that it can be safely discarded. Also, since the amount of negative registries is much greater than the positive, I'll weight the samples so that they're balanced in order to avoid instability on the models.

After doing this with a tree of a maximum depth of five, for visualisation purposes, we can notice that the only variables that appear are  'estrato','productos', 'portafolio' and 'antiguedad_meses'; towards the bottom we start seeing sometimes 'tipo_fuerza_venta' and other binary variables like 'asesoria_servicios', 'quejas_fraude' or 'no_serv_tecnicos'. The first five will be included in the model. As for the other binary variables, I will include them since they have information about the interaction between the customer and the company, which the latter can monitor more efficiently if needed.

## Logistic regression

The model I will choose is a logistic regression. Since we are looking for the probability of belonging to a positive class, this method seems the most natural to use, also, we want to be able to know which variables have greater impact on said probability, which is easier do with a logistic regression compared to a neural network.

For the model, we need to scale the only non binary variable: 'antiguedad_meses', since its range goes from 0 to 337, the algorithm will have a hard time finding optimal weights for all the variables, so I divided it by its maximum value and then trained. As with the decision tree, weights were introduced for algorithm stability.

In the end, the classifier performed well. Although its accuracy was of 0.6676, its recall was 0.7832, meaning that about 4 of every 5 nonpayers was identified.

Now, after calculating the probabilities for all the clients, I started varying the threshold for the low, medium and high risk classes on the excel file. After trying different values, and looking at the proportions of the classes on them, I decided to use the values of 0.5 and 0.8 as the limits. In this scenario, 348 people would be scheduled for a call, and only 159 would actually be done. In total, the cost of the whole strategy would be 378140.

## Risky profiles

For finding the profiles that have the most and least risk, I look at the weights and their size and sign: positive weights point towards a risky client, whereas negative ones point towards safety; a great size in the weight means a great impact. So I look at them sorted by their absolute value and look out for the positives. With this, we can see that the ones that make a user risky are them belonging to estrato 1, 2, 4 or 3, having a direct sales force type, acquiting a TO+TV+BA product and having a trio portfolio. Interestingly enough, estrato 4 has a greater weight than estrato 3 even though theoretically people in the former have more money available than people in the latter. Likewise, the profile of a low risk client would be someone with an estrato of SE, who has asked for technical services, has complained about fraud, had an indirect sales force type, and an individual portfolio. Finally, it is worth noting that, since 'antiguedad_meses' has a negative weight, a new client is much more likely to not pay than a client that has had the services of the company for some time.

### Suggestions and conclusions

As we saw thoughout this exercise, the clients that are most liely to not pay are those that acquire TO+TV+BA, have a trio portfolio and are contacted directly, while belonging to certain strata. Now, since the stratum is something the company cannot control, changing the policies of the TO+TV+BA and the trio portfolio to be more fraud-blinded might help, as well as financing alternatives aimed towards these groups in order to tackle this problems.