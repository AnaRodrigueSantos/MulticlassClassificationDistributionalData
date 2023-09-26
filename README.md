# MulticlassClassificationDistributionalData
Code used for the thesis: Multi-class Classification for Distributional Data

Esta nota explica 2 funções fundamentais para usar o código
desenvolvido. Note-se que há restantes funções que não estão 
explicadas e servem de suporte à aplicação do modelo.

Funções:

1) TrainTestSplit:
	Input:
		data: conjunto de dados simbólicos (dataset). 
		groups_vector: vetor numérico com a classificação multi classe a priori 
		test_size: valor numérico 0-1 com a proporção de dados que se pretende para teste.
			valor por omissão (default): 0.2
		train_size: valor numérico 0-1 com a proporção de dados que se pretende para treino.
			valor por omissão (default): 0.8

	Output:
		lista com elementos pela seguinte ordem:
			{conjunto de treino, classificação a priori do conjunto de treino,
			conjunto de teste, classificação a priori do conjunto de teste} =
			= {trainSet, trainGroups, testSet, testGroups}



2) LDAClassification:
	Input:
		Traindata: conjunto de dados de treino. 
		Traingroups: vetor numérico com a classificação multi classe a priori associada ao conjunto de treino.              
		Testdata: conjunto de dados de teste.
		Testgroups: vetor numérico com a classificação multi classe a priori associada ao conjunto de teste.
                classification_technique: string entre as seguintes opções, consoante o método que se prentende aplicar
			"CLDF with weights", "OVO", "OVA", "CLDF without weights"
                print_prediction: TRUE/FALSE, TRUE para dar print da classificação obtida pelo método.

	Output:
		vetor numérico com a classificação multi classe.



Notas:

1) Para obter tempos de execução usar: tic() ... toc(log = TRUE)

2) A "ground truth", classificação a priori, é suposto
ser um vetor numérico. Se não for o caso, sugere-se a seguinte 
linha de código: groups_vector <- as.numeric(groups_vector)

3) Para saber a quantidade de unidades que pertencem a cada classe 
no conjunto de treino e teste sugere-se a utilização da função "table"
sobre o vetor numérico com a classificação a priori.
exemplo:
table(train_groups)
table(test_groups)




Exemplo de aplicação:
```{r teste: exemplo de código}
load(file = 'Balanced.Rdata')
load(file = 'BalancedGroups.Rdata')
groups_vector <- as.numeric(factor(groups_vector))
table(groups_vector)

d <- TrainTestSplit(DadosRedes, groups_vector, test_size=0.2, train_size=0.8)
train <- d$trainSet
train_groups <- d$trainGroups
test <- d$testSet
test_groups <- d$testGroups
table(train_groups)
table(test_groups)


PredClassMulti <- LDAClassification(train, train_groups, 
                             test, test_groups, 
                             classification_technique="CLDF without weights", 
                             print_prediction=TRUE)

```
