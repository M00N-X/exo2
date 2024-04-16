# exo2
# NOM: ANDRIAMANANA 
# PRENOM : VOLATIANA ELSA
# NUMEROS D'INSCRIPTION :
# MI  L1
# E-MAIL: elsaandriamanana15@gmail.com
def generer_table_de_verite(nombres_variables, fonction_logique, variables):
    table_verite = []
    for i in range(2**nombres_variables):
        combinaison = []
        for j in range(nombres_variables):
            combinaison.append((i >> j) & 1)
        resultat = eval(fonction_logique, dict(zip(variables, combinaison)))
        table_verite.append((combinaison, resultat))
    return table_verite

  def trouver_groupes_adjacents(table_verite, nombres_variables):
    groupes = []
    for i in range(nombres_variables):
        nouveaux_groupes = []
        for groupe in groupes:
            for ligne in table_verite:
                if sum(abs(x - y) for x, y in zip(groupe[0], ligne[0])) == 1:
                    if not any(all(x == ligne[0][j] for j, x in enumerate(l)) for l in groupe[0]):
                        nouveaux_groupes.append((groupe[0] + [ligne[0]], groupe[1]))
        groupes += nouveaux_groupes
    return groupes

  def karnaugh_simplification(groupes, variables):
    resultats_simplifies = []
    for groupe in groupes:
        termes = []
        for i, valeur in enumerate(groupe[0][0]):
            if valeur == 1:
                termes.append(variables[i])
            elif valeur == 0:
                termes.append(f"not {variables[i]}")
        resultats_simplifies.append("(" + " and ".join(termes) + ")")

    if resultats_simplifies:
        print("Forme simplifiée de la fonction en utilisant la méthode de Karnaugh :")
        print(" or ".join(resultats_simplifies))
    else:
        print("La fonction est déjà simplifiée et ne peut pas être davantage simplifiée en utilisant la méthode de Karnaugh.")

  def principale():
    nombres_variables = int(input("Entrez le nombre de variables: "))
    variables = []
    for i in range(nombres_variables):
        nom_variable = input(f"Entrez le nom de la variable {i+1}: ")
        variables.append(nom_variable)
    fonction_logique = input("Entrez la fonction logique (en utilisant les noms des variables et les opérateurs logiques (and, not, or)): ")
    table_verite = generer_table_de_verite(nombres_variables, fonction_logique, variables)
    groupes = trouver_groupes_adjacents(table_verite, nombres_variables)
    karnaugh_simplification(groupes, variables)

principale()
