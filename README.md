1. **Resposta correcta:** a. Són uns fitxers de mapatge per relacionar les classes Java a persistir en taules d’una base de dades relacional.  
Explicació: Els fitxers `.hbm.xml` són utilitzats en Hibernate per mapar les classes Java amb les taules de la base de dades.

2. **Resposta correcta:** d. Principalment ens garanteix que una classe només tingui una instància, i ens proporciona un punt d'accés global a ella.  
Explicació: El patró Singleton assegura que una classe tingui només una instància, i aquesta instància serà accessible des de qualsevol part del programa.

3. **Resposta correcta:** d. Que el mapeig amb la taula SQL `products` sols es fa mitjançant el fitxer `products.hbm.xml`.  
Explicació: La configuració `<mapping resource="products.hbm.xml"/>` especifica que la classe `products` està associada a un fitxer XML per al mapeig de la taula de la base de dades.

4. **Resposta correcta:** b. Que els valors que prengui no es mapejaran en cap cas a la taula corresponent.  
Explicació: L'anotació `transient` impedeix que l'atribut es mapegi a la base de dades.

5. **Resposta correcta:** a. FetchType.LAZY carrega les dades de forma diferida, mentre que FetchType.EAGER les carrega de manera immediata.  
Explicació: `FetchType.LAZY` carrega les dades només quan es necessiten, mentre que `FetchType.EAGER` carrega les dades immediatament quan s'accedeix a l'entitat.

6. **Resposta correcta:** d. Que hem fet `session.find(tasks.class, "7");`  
Explicació: La consulta SQL mostra una selecció de la taula `tasks`, indicant que l'objecte amb id 7 és recuperat (probablement a través d'una consulta).

7. **Resposta correcta:** b. persist()  
Explicació: A partir de Hibernate 6, `persist()` és el mètode recomanat per guardar una nova entitat en el context de persistència.

8. **Resposta correcta:** b. hbm2ddl.auto  
Explicació: La propietat `hbm2ddl.auto` controla la creació o modificació automàtica de la base de dades en funció de les entitats definides.

9. **Resposta correcta:** b. Query<Vehicle> q = sesion1.createQuery(“Select c from Vehicle c where c.marca = “Seat””, Vehicle.class); q.setFetchSize(50);  
Explicació: El mètode `setFetchSize(50)` especifica el nombre de registres que es recuperaran en cada "bloc".

10. **Resposta correcta:** b. L’anotació `JoinColumn` permet indicar el nom de la columna que serà clau forana.  
Explicació: L’anotació `@JoinColumn` es fa servir per definir la columna de la taula que serà utilitzada com a clau forana.

### Exercicis:
**a. Escriviu les propietats i anotacions per a l'entitat `Tests`:**
```java
@Entity
@Table(name="tests")
public class Tests implements Serializable {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long idTest;
    private String descTest;
    private Integer numPreguntesTest;
    
    @OneToMany(mappedBy = "test", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private Set<Preguntes> preguntes;
}
```

**b. Completa la relació entre `Tests` i `Preguntes`:**
```java
@Entity
@Table(name="tests")
public class Tests implements Serializable {
    // definicions segons l’exercici de dalt
    
    @OneToMany(mappedBy = "test", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private Set<Preguntes> preguntes;
}

@Entity
@Table(name="preguntes")
public class Preguntes implements Serializable {
    // definicions segons l’exercici de dalt
    
    @ManyToOne(cascade = CascadeType.ALL)
    @JoinColumn(name="idTest", foreignKey = @ForeignKey(name="FK_TEST"))
    private Tests test;
}
```

**c. Completar el codi per afegir noves preguntes:**
```java
public class App { 
    public static void main( String[] args ) { 
        SessionFactory sesion = HibernateUtil.getSessionFactory(); 
        Session session = sesion.openSession(); 
        session.beginTransaction(); 
        
        Test t1 = new Test("Test de coneixements de música", 30); 
        Pregunta p1 = new Pregunta("Quin any va néixer Mozart?", "a. 1756", "b. 1800", "c. 1789", "d. 1956", "a. 1756", t1); 
        
        session.save(p1); 
        
        session.getTransaction().commit(); 
        session.close(); 
    } 
}
```

**d. Consulta HQL per obtenir el valor mitjà del nombre de preguntes per test:**
```java
Query<Double> query = session.createQuery(
    "SELECT avg(t.numPreguntesTest) FROM Tests t", Double.class);
valorMig = query.getSingleResult();
```
