#include <stdio.h>
#include <stdlib.h>

// تعريف بنية العقدة في القائمة المرتبطة
typedef struct element {
    int val;
    struct element *suivant;
} element;

// 1. دالة لإنشاء قائمة فارغة
element* creerListe() {
    return NULL;
}

// 2. دالة لشحن/ملء القائمة من جدول (مصفوفة)
element* chargerListe(int Tab[], int taille, element *liste) {
    for (int i = 0; i < taille; i++) {
        // إنشاء عنصر جديد
        element *nouv = (element*) malloc(sizeof(element));
        if (nouv == NULL) {
            printf("Erreur d'allocation memoire!\n");
            exit(1);
        }
        nouv->val = Tab[i];
        nouv->suivant = NULL;

        // إذا كانت القائمة فارغة، يصبح العنصر الجديد هو رأس القائمة
        if (liste == NULL) {
            liste = nouv;
        } else {
            // الإضافة في نهاية القائمة للحفاظ على ترتيب الجدول
            element *temp = liste;
            while (temp->suivant != NULL) {
                temp = temp->suivant;
            }
            temp->suivant = nouv;
        }
    }
    return liste;
}

// 3. إجراء لعرض عناصر القائمة بالتنسيق المطلوب
void afficherListe(element *L) {
    element *temp = L;
    while (temp != NULL) {
        printf("%d", temp->val);
        if (temp->suivant != NULL) {
            printf(" -> ");
        }
        temp = temp->suivant;
    }
    printf(" -> NULL\n");
}

// 4. دالة لحذف العنصر الأخير من القائمة
element* supprimerEnFin(element *L) {
    if (L == NULL) {
        return NULL; // القائمة فارغة أصلاً
    }
    // إذا كانت القائمة تحتوي على عنصر واحد فقط
    if (L->suivant == NULL) {
        free(L);
        return NULL;
    }
    
    element *temp = L;
    while (temp->suivant->suivant != NULL) {
        temp = temp->suivant;
    }
    // حذف العنصر الأخير
    free(temp->suivant);
    temp->suivant = NULL;
    
    return L;
}

// 5. دالة لإضافة عنصر في بداية القائمة
element* ajouterEnDebut(element *L1, int valeur) {
    element *nouv = (element*) malloc(sizeof(element));
    if (nouv == NULL) {
        printf("Erreur d'allocation memoire!\n");
        exit(1);
    }
    nouv->val = valeur;
    nouv->suivant = L1; // الربط بالعقدة الأولى القديمة
    return nouv;        // العنصر الجديد أصبح هو رأس القائمة
}

// 6. إجراء لتفريغ القائمة وحذف جميع العقد من الذاكرة
void viderListe(element *L2) {
    element *temp;
    while (L2 != NULL) {
        temp = L2;
        L2 = L2->suivant;
        free(temp);     
    }
    printf("The list is empty.\n");
}

// الدالة الرئيسية المعطاة في التمرين
int main() {
    int Tab[10] = {1, 3, 5, 7, 8, 10, 9, 11, 13, 20};
    
    element *liste = creerListe();
    element *L = chargerListe(Tab, 10, liste);
    afficherListe(L);
    
    element *L1 = supprimerEnFin(L);
    afficherListe(L1);
    
    element *L2 = ajouterEnDebut(L1, 40);
    afficherListe(L2);
    
    viderListe(L2);
    
    return 0;
}   
