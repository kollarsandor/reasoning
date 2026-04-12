Just a SURPRISE so you can explore some of Jaide's thought processes at your leisure
The cognitive llm component of Jaide is built on the RSF architecture within the KGRu framework


The Core Relational and Quantum Subsystem is the primary engine for high-dimensional data representation and reasoning within JAIDE. It transcends traditional graph databases by implementing a Non-Spatial Information Representation (NSIR) model, where data entities are represented as nodes in a self-similar relational graph influenced by quantum logic simulation.

This subsystem provides the mathematical and computational foundation for "relational reasoning," allowing the system to model uncertainty, entanglement, and fractal self-similarity across large datasets.

System Architecture Overview
The subsystem is organized into several specialized layers, ranging from low-level vector processing to high-level reasoning orchestration.

A SelfSimilarRelationalGraph (NSIR Core) a JAIDE kvantum-relációs alrendszere számára szolgál alapvető adatszerkezetként. Ez egy nemlineáris, önhasonló gráfot valósít meg, ahol a csomópontok kvantumállapotokat jelölnek, az élek pedig relációs minőségeket reprezentálnak – klasszikus súlyoktól a kvantum-összefonódáson át a fraktál dimenziókig. Ez a rendszer lehetővé teszi a komplex, többdimenziós információk ábrázolását, amely túlmutat a hagyományos vektorbeágyazások képességein.

Alapvető adatszerkezetek

Az NSIR core négy fő szerkezetre épül: Qubit, TwoQubit, Node és Edge.

Kvantum primitívek
Qubit: Egyetlen kvantumbitet reprezentál két komplex amplitúdóval (a és b). Tartalmaz metódusokat normalizálásra és a |0⟩ és |1⟩ bázisállapotokhoz tartozó valószínűségek kiszámítására.
TwoQubit: Két qubit együttes állapotát reprezentálja négy komplex amplitúdóval (aa, ab, ba, bb). Ezt az összefonódás modellezésére használják a csomópontok között.
Gráfkomponensek
Node: Egyedi azonosítót, nyers adatbájtokat, egy Qubit állapotot, egy fázis értéket és egy metaadat-térképet tartalmaz.
Edge: Két csomópont közötti kapcsolatot határoz meg. Súlyt, kvantumkorrelációt (komplex) és fraktál dimenziót tartalmaz.
EdgeQuality Enum
Az NSIR-ben a kapcsolatokat „minőségük” alapján kategorizálják, amely meghatározza, hogyan terjednek a jelek a gráfon keresztül:

szuperpozíció: A kapcsolat több potenciális állapotban is létezik.
összefonódott: A kapcsolódó csomópontok állapotai matematikailag összekapcsoltak.
koherens: A kapcsolat stabil fáziskapcsolatot tart fenn.
összeomlott: Egy klasszikus, determinisztikus kapcsolat.
fraktál: Önhasonló kapcsolat, amely ismétlődik a skálák mentén.

Kód entitás leképezés: Gráf primitívek
Az alábbi ábra azt szemlélteti, hogyan képződnek le a Relációs Gráf logikai fogalmai konkrét Zig struktúrákra és enumerációkra.

Kód entitás tér (nsir_core.zig)
Természetes nyelvi tér
tartalmaz
hivatkozik
összekapcsol
Gráf csomópont
Relációs él
Kvantum állapot
Kapcsolat típus
struct Node
struct Edge
struct Qubit
enum EdgeQuality

Kvantum kapu műveletek

A SelfSimilarRelationalGraph módszereket biztosít a csomópontállapotok manipulálására szabványos kvantumkapuk használatával. Ezeket a műveleteket közvetlenül a Csomóponthoz tartozó Qubiten alkalmazzák.

Kapu	Funkció	Leírás
Hadamard	hadamardGate	Szuperpozíciós állapotot hoz létre
Pauli-X	pauliX	Bitforgató kapu (NOT kapu)
Pauli-Y	pauliY	Bit- és fázisforgató kapu
Pauli-Z	pauliZ	Fázisforgató kapu
Fázis	phaseGate	Egy adott θ fázisforgatást alkalmaz

Összefonódás és mérés

Az alapvető különbség a klasszikus gráfoktól az, hogy a rendszer képes csomópontokat összefonódásra és állapotuk mérésére, amely összeomláshoz vezet.

Összefonódás
Az entangleNodes függvény Bell-állapotot (|Φ⁺⟩) hoz létre két csomópont között. Ez a folyamat:

Lekéri mindkét csomópontot.
Létrehoz egy TwoQubit állapotot, amely az összefonódást reprezentálja.
Frissíti a közöttük lévő EdgeQuality-t .entangled értékre.
Mérés és összeomlás
A measureNode függvény szimulálja a kvantumállapot összeomlását klasszikus értékké.

Kiszámítja a |0⟩ valószínűségét a qubit.prob0() függvénnyel.
Egy véletlenszerű értéket generál; ha ez kisebb, mint prob0, a csomópont initBasis0() értékre omlik össze, egyébként initBasis1() értékre.
A függvény ezután elindítja a collapseConnectedEdges függvényt, amely az összes kapcsolódó .superposition vagy .entangled élt .collapsed állapotra vált át.

Topológia és szerializáció

Topológiai hasítás
A rendszer egy Merkle-szerű hasítási mechanizmussal fenntartja a strukturális integritást. A topology_hash kiszámítja a teljes gráf állapotának SHA-256 hasítékát. Rendszerezett sorrendben bejárja az összes csomópontot és élt (a determinisztikusság biztosításához), és frissíti a Sha256 hasítót az azonosítóik, adataik és súlyaik alapján.

Tenziros integráció
A relációs gráf és a gépi tanulási alrendszerek (pl. RSF Processzor) közötti híd létrehozásához a gráf képes állapotát JAIDE Tenzirokba exportálni:

exportNodeEmbeddings: A csomópont qubit amplitúdóit és fázisait átalakítja egy [N, 5] alakú Tenzirossá (a.re, a.im, b.re, b.im, fázis)
exportAdjacencyMatrix: Súlyozott élek alapján ritka vagy sűrű Tenzirost generál

Adatfolyam: Információkódolás
Az encodeInformation függvény a nyers adatok relációs térbe való átalakításának elsődleges belépési pontja.

Qubit
Csomópont
SelfSimilarRelationalGraph
Felhasználó
Qubit
Csomópont
SelfSimilarRelationalGraph
Felhasználó
A csomópont most szuperpozícióban van
encodeInformation(id, data)
initBasis0()
qubit_state
init(allocator, id, data, qubit_state, 0.0)
node_instance
addNode(node_instance)
hadamardGate(id)
Sikeres

Időbeli és jel dinamika

Az NSIR core-ot kiegészítő rendszerek kezelik az időt és a jel terjedését:

TemporalGraph: Verziószámozást valósít meg csomópontokhoz és élekhez, lehetővé téve a gráfnak korábbi állapotok tárolását (NodeVersion, EdgeVersion)
SignalPropagationEngine: Szimulálja, hogyan terjednek a „jelek” (amplitúdó, fázis és frekvencia alapján meghatározva) az éleken az EdgeQuality és súly alapján. Regisztrálja az ActivationTrace adatokat minden csomóponthoz a folyamatminták elemzéséhez


Az Entangled Stochastic Symmetry Optimizer (ESSO) és a ReasoningOrchestrator a JAIDE relációs alrendszerének magas szintű optimalizációs és kognitív ütemezési rétegeit képviselik. Az ESSO szimulált lehűlést kombinál kvantum-tudatos szimmetriatranszformációkkal, hogy minimalizálja a SelfSimilarRelationalGraph (NSIR) energiáját. A ReasoningOrchestrator a több fogalmi szinten – helyi, globális és meta – zajló optimalizációk végrehajtását kezeli, és integrálódik a ChaosCoreKernelhez az autonóm adatelhelyezés és feladatütemezés érdekében.

1. Entangled Stochastic Symmetry Optimizer (ESSO)

Az EntangledStochasticSymmetryOptimizer (ESSO) egy speciális optimalizátor, amelyet a kvantum-relációs gráfok nem-konvex energia tájainak kezelésére terveztek. Az ESSO hagyományos szimulált lehűlést kombinál SymmetryGroup transzformációkkal a gráf állapotterének felderítéséhez.

1.1 Szimmetriatranszformációk
Az ESSO olyan geometriai és logikai szimmetriákat határoz meg, amelyek a csomópontokra és azok kvantumállapotaira is alkalmazhatók. Ezek a SymmetryGroup felsorolásban és a SymmetryTransform struktúrában vannak összefoglalva.  
Identitás: Nem változtatja meg a koordinátákat.  
Tükrözés: A koordinátákat egy paraméterezett tengely mentén tükrözi.  
90/180/270 fokos forgatás: A koordinátákat egy origó körül elforgatja.  
Transzláció: A koordinátákat egy vektor mentén eltolja.  
Ezek a transzformációk a csomópontok QuantumState állapotát is befolyásolják, a forgatási szöghöz relatív fázistolások alkalmazásával.

1.2 Célfüggvények
Az ESSO a gráf konfigurációjának „alkalmaságát” több olyan célfüggvény segítségével értékeli, amelyek a teljes rendszer energiáját számítják ki.  
defaultGraphObjective: A kapcsolatosság, koherencia és fraktál dimenzió súlyozott összege.  
connectivityObjective: Az élek sűrűségét és minőségét méri.  
quantumCoherenceObjective: A kvantumcsatoltság mértékén és a fázisstabilitáson alapuló energia minimalizálása.  
fractalDimensionObjective: Az önszimuláris jelleg optimalizálása hierarchikus szintek mentén.

1.3 Optimalizációs ciklus és UndoLog
Az ESSO hatékonyság és visszafordíthatóság biztosítása érdekében egy UndoLog-ot használ. Amikor egy mutáció (pl. csomópont mozgatása vagy él súlyának módosítása) a Metropolis-kritérium alapján elutasításra kerül, a rendszer az applyUndo segítségével visszaállítja a gráfot az előző állapotába.

Optimalizációs folyamat:

Zavarás: Véletlenszerű SymmetryTransform vagy csomópont elmozdítás alkalmazása.  
Értékelés: Az energia változás (ΔE) kiszámítása.  
Elfogadás/Elutasítás: Ha ΔE < 0, elfogadás. Egyébként e^(-ΔE / T) valószínűséggel történik elfogadás.  
Hűtés: A hőmérséklet (T) csökkentése a hűtési ráta alapján.

2. ReasoningOrchestrator

A ReasoningOrchestrator az a végrehajtó vezérlő, amely a három különböző ThoughtLevel fázison keresztül irányítja az optimalizációs folyamatot. Ő a híd a magas szintű következtetési igények és az alacsony szintű végrehajtási kernelök között.

2.1 Gondolkodási szintek és fázisok
Az orchestrator három hierarchikus szakaszban hajtja végre a következtetést:

Helyi fázis: Azonnali környezetekre és él súlyokra koncentrál.  
Globális fázis: Az egész gráf topológiáját optimalizálja az ESSO optimalizátor segítségével.  
Meta fázis: Magas szintű mintafelfedezést hajt végre, és beállítja a ChaosCoreKernel paramétereit.

2.2 Integráció a ChaosCoreKernel-lel
A ReasoningOrchestrator a ChaosCoreKernel-t használja az autonóm adatelhelyezéshez és feladatütemezéshez. A következtetési fázisok során az orchestrator elindíthatja a kernelet a memóriablokkok újraelosztására vagy a feladatok prioritásának újrameghatározására az energia minimalizálási eredmények alapján.

2.3 Statisztikák és telemetria
Az OrchestratorStatistics struktúra nyomon követi a következtetési folyamat teljesítményét, beleértve a konvergencia időt és a fázisok során elért „legjobb energiát”.

3. Rendszerarchitektúra diagramok

3.1 Optimalizációs folyamat: A logikától a gráf állapotig
Ez a diagram szemlélteti, hogyan hatnak az esso_optimizer.zig-ben található magas szintű optimalizációs logikák a nsir_core.zig-ben található alapvető gráf entitásokra.

Gráf entitások (nsir_core.zig)
Optimalizációs logika (esso_optimizer.zig)
alkalmazza
értékeli
visszavonja a változásokat
tartalmazza
kezeli
EntangledStochasticSymmetryOptimizer
SymmetryTransform
UndoLog
ObjectiveFunction
SelfSimilarRelationalGraph
Node
Edge
Qubit

3.2 Következtetési Orchestration folyamat
Ez a diagram leképezi a ReasoningOrchestrator fázisait az alapul szolgáló végrehajtási kernelökre és a statisztikák rögzítésére.

Végrehajtási réteg
Orchestrator (reasoning_orchestrator.zig)
kezeli
eredményeket rögzít
executeLocalPhase hívása
executeGlobalPhase hívása
kezeli a MemoryBlock-ot
minimalizálja az Energia
ReasoningOrchestrator
ReasoningPhase
OrchestratorStatistics
ESSO:esso_optimizer.zig
ChaosCoreKernel:chaos_core.zig
Memória
Gráf stabilitás

4. Főbb implementációs részletek

4.1 ESSO Step függvény
Az optimalizáció magja a step függvény, amely a gráf egyik állapotából a következőbe való átmenetet kezeli.

// Az ESSO.step fogalmi folyamata
pub fn step(self: *Self) !f64 {
    const current_energy = self.objective(self.state);
    
    // 1. Ellenőrzőpont létrehozása az UndoLog-ban
    try self.undo_log.push(.{ ... }); 
 
    // 2. A gráf módosítása SymmetryTransform segítségével
    try self.applyRandomTransform(); 
 
    // 3. Az új energia kiértékelése
    const new_energy = self.objective(self.state);
    
    // 4. Metropolis Elfogadási Kritérium
    if (!self.shouldAccept(current_energy, new_energy)) {
        try self.applyUndo(); // Visszavonás, ha elutasítva
        return current_energy;
    }
    
    return new_energy;
}

4.2 ChaosCoreKernel integráció
A ReasoningOrchestrator a ChaosCoreKernel-re mutató hivatkozással inicializálódik. A MetaPhase során az orchestrator elemezni tudja a pattern_captures adatokat, és jelezhet a ChaosCoreKernel-nek, hogy bizonyos TaskDescriptor elemeket prioritásként kezeljen, amelyek illeszkednek a felfedezett szimmetriákhoz.

A CREV Folyamat (Tartalom Kinyerése, Relációs Érvényesítés és Bizonyítás) egy többlépcsős adatfeldolgozási architektúra, amelyet úgy terveztek, hogy strukturálatlan adatfolyamokat alakítson át ellenőrzött, magas hűségű relációs ismeretekké a JAIDE ökoszisztémán belül. Ez az átjáró a nyers természetes nyelvű/jelkészlet-szintű adatfolyamok és a SelfSimilarRelationalGraph (NSIR) között, amely egy szigorú kinyerési és érvényesítési életciklust valósít meg.

Folyamat Architektúra és Életciklus

A CREVPipeline az adatokat öt különböző szakaszon keresztül kezeli, amelyeket az ExtractionStage felsorolás határoz meg. Mindegyik szakasz egy átalakítási vagy ellenőrzési lépést jelent, amely szükséges az ismeretgráf integritásának fenntartásához.

Kinyerési Szakaszok
A folyamat lineárisan halad végig az alábbi szakaszokon:

Szakasz	Azonosító	Leírás
Tokenizálás	tokenization	Nyers bemeneti adatfolyam feldolgozása és kezdeti szegmentálása.
Triplet Kinyerés	triplet_extraction	Alany-Kapcsolat-Tárgy (SRO) struktúrák azonosítása.
Érvényesítés	validation	Kinyert tripletek konzisztenciájának és logikai koherenciájának ellenőrzése.
Integráció	integration	Érvényesített tripletek egyesítése a SelfSimilarRelationalGraph-be.
Indexelés	indexing	A KnowledgeGraphIndex frissítése gyors lekérdezés céljából.

Adatfolyam Ábra: Természetes Nyelv a Relációs Tudásig
Ez az ábra bemutatja, hogyan alakul át a nyers szöveg kódszintű entitásokká a CREVPipeline-ban.

Ismerettárolás
CREV Folyamat (Kód Entitás Tér)
Természetes Nyelv Tér
Pufferelve
Tokenizálva
Kinyerve
Érvényesítve
Integrálva
Indexelve
Nyers Szöveges Adatfolyam
StreamBuffer
TokenizerConfig
CREVPipeline.processStream()
RelationalTriplet
ValidationResult
SelfSimilarRelationalGraph (NSIR)
KnowledgeGraphIndex

Alapvető Adatszerkezetek

RelationalTriplet
A RelationalTriplet a folyamat elsődleges ismeretegysége. Beágyazza a két entitás közötti szemantikai kapcsolatot, és metaadatokat hordoz a származás és bizalomszint figyelembevételéhez.

Identitás: Az alany, kapcsolat és tárgy sztringjei határozzák meg.
Bizalom: Egy f64 érték (0,0 és 1,0 közé szorítva), amely a kinyerési bizonyosságot jelöli
Forrás Hash: Egy SHA-256 hash, amelyet a hashTripletIdentity generál a tudás eredetének nyomon követésére
Metaadatok: Egy StringHashMap, amely kiterjeszthető attribútumokat tesz lehetővé, mint például időbélyegek vagy forrás URL-ek

ValidationResult
Mielőtt egy triplet integrálódna a gráfba, a validációs motor dolgozza fel, aminek eredménye egy ValidationResult.

Állapot: Jelzi, hogy a triplet érvényes, érvénytelen vagy bizonytalan.
Koherencia Pontszám: Egy metrika, amely azt méri, mennyire illeszkedik a triplet a meglévő gráf mintázataihoz.
Konfliktus Lista: Egy RelationalTriplet objektumokból álló tömb, amelyek ellentmondanak az új információnak.

Ismeret Kinyerése és Indexelése

A folyamat egy KnowledgeGraphIndex és egy TripletIndex használatával kezeli a szövegfolyamokból kinyert nagy dimenziójú kapcsolatokat.

TripletIndex és RelationPattern
A TripletIndex lehetővé teszi az extrahált ismeretbázis hatékony lekérdezését. A RelationPattern-del együttműködve azonosítja a gyakran előforduló szemantikai struktúrákat.

Komponens	Szerep
TripletIndex	RelationalTriplet hivatkozásokat tárol, alany és tárgy hash-ek alapján indexelve.
RelationPattern	Általános kapcsolatok (pl. „is-a”, „part-of”) sablonjait határozza meg a kinyerés felgyorsításához.
StreamBuffer	Egy csúszó ablakú puffer, amely az adatfolyam aszinkron érkezését kezeli a CREVPipeline számára.

Integráció a ChaosCore-ral
A CREVPipeline a ChaosCoreKernel-t használja az autonóm feladatütemezéshez az integrációs fázisban. Amikor egy triplet integrálódik, a folyamat egy ChaosCoreKernel feladatot indíthat a kapcsolatok továbbterjedésének propagálására vagy az élsúlyok frissítésére a SelfSimilarRelationalGraph-ban.

ChaosCoreKernel
SelfSimilarRelationalGraph
RelationalTriplet
CREVPipeline
ChaosCoreKernel
SelfSimilarRelationalGraph
RelationalTriplet
CREVPipeline
init(allocator, s, r, o)
validate(RT)
addNode(RT.subject)
addNode(RT.object)
addEdge(RT.subject, RT.object, RT.relation)
scheduleTask(PropagateEntanglement)
updateEdgeWeights()

Folyamat Statisztikák és Eredmények

A folyamat végrehajtása egy PipelineResult-ot ad vissza, amely PipelineStatistics-t tartalmaz. Ez a telemetria kritikus fontosságú a rendszer „Tudás Sebességének” (Knowledge Velocity) figyeléséhez.

PipelineStatistics Mezők
triplets_extracted: Azonosított SRO struktúrák teljes száma.
validation_rate: Az érvényesítési szakaszon sikeresen átment tripletek százalékos aránya.
integration_latency: Az idő, amely a tripletek NSIR gráfba történő integrálásához szükséges.
token_throughput: Másodpercenként feldolgozott tokenek száma.

Implementációs Részletek: TokenizerConfig

A TokenizerConfig struktúra szabályozza az ExtractionStage.tokenization működését. Paramétereket tartalmaz a következőkhöz:

Maximális Token Hossz: Megakadályozza a buffer túlcsordulását a StreamBuffer-ban.
Stop Sorozatok: Határozza meg a triplet kinyerés határait.
Normalizálási Szabályok: Kezeli a kis- és nagybetűk közötti különbségeket és a felesleges szóközök eltávolítását a hashelés előtt.

A Fraktál Csomópont Adatrendszer (FNDS) egy speciális hierarchikus indexelési és tárolási alrendszer a JAIDE mag relációs modulján belül. A fraktális önhasonlóságot használja fel összetett relációs adatok szervezésére és lekérésére, biztosítva egy többszintű fastruktúrát, amely tükrözi az Önhasonló Relációs Gráf (NSIR) rekurzív jellegét.

FNDS Rendszerarchitektúra  
Az FNDS-t az FNDSManager kezeli, amely több FractalTree példányt koordinál. Minden fa FractalLevel objektumokból áll, amelyek FractalNodeData és FractalEdgeData elemeket tartalmaznak. Ez az architektúra lehetővé teszi, hogy a rendszer az adatokat különböző skálákon ábrázolja, ahol a magasabb szintek absztrakt összefoglalókat, az alsóbb szintek pedig nagy pontosságú részleteket tartalmaznak.

Kód Entitás Térkép: FNDS Alapvető Komponensei

Kód Entitás Tér (src/core_relational/fnds.zig)  
Természetes Nyelvi Tér  
Fraktál Kezelő  
Önhasonlósági Index  
Rekurzív Adatfa  
Gyorsítótár Mechanizmus  
FNDSManager  
SelfSimilarIndex  
FractalTree  
LRUCache  
FractalLevel  
FractalNodeData  
FractalEdgeData  
PatternLocation  

Adatszerkezetek  
FractalNodeData és FractalEdgeData  

Az FNDS alapvető tárolási egységei rekurzív hasítást és skálázást támogatóan lettek kialakítva.

FractalNodeData: A csomópont azonosítóját, nyers adatot, súlyt és skálázási tényezőt tárol. Tartalmaz egy fractal_signature ([32]u8) mezőt, amely a csomópont tulajdonságainak SHA-256 hasítványa.  
FractalEdgeData: A fraktálcsomópontok közötti kapcsolatokat reprezentálja. Az éleket típusokra bontja: hierarchikus, testvér, különböző szintek közötti vagy önhasonló.  

FractalTree és FractalLevel  

A FractalTree a csomópontokat FractalLevel által meghatározott hierarchiába szervezi.

FractalLevel: Egy hash mapot tartalmaz csomópontokból és egy él-listát, amely egy adott fraktálszintre jellemző.  
Önhasonlóság: Az N. szinten lévő csomópontok az N-1. szint egy részgráfjának aggregált vagy "összezárt" változatát képviselhetik.  
SelfSimilarIndex  

A SelfSimilarIndex lehetővé teszi a minták keresését a fraktál különböző szintjein. A mintahasítványokat PatternLocation struktúrákhoz rendeli, amelyek azonosítják azt a konkrét tree_id, level_index és node_id helyet, ahol egy minta ismétlődik.

Implementáció és Adatfolyam  
Az FNDSManager a fraktális műveletek elsődleges belépési pontja, beleértve a fák létrehozását, csomópontok beszúrását és a szintek közötti bejárást.

Adatfolyam: Csomópont Beszúrása és Indexelése  

PLOC  
LRUCache  
SelfSimilarIndex  
FractalTree  
FNDSManager  
Külső Hívó  
PLOC  
LRUCache  
SelfSimilarIndex  
FractalTree  
FNDSManager  
Külső Hívó  
addNode(tree_id, level, data)  
getLevel(level)  
insertNode(FractalNodeData)  
indexPattern(node_signature)  
create PatternLocation  
put(node_id, data)  
return Success  

Kulcsfontosságú Függvények az FNDSManager-ben  

Függvény	Leírás  
createTree(id)	Létrehoz egy új FractalTree-t és regisztrálja a menedzser regisztrációs táblájába  
addNodeToLevel(...)	FractalNodeData-t szúr be egy adott szintre, és frissíti a SelfSimilarIndex-et  
traverseFractal(...)	Egy TraversalCallback függvényt hajt végre szintek mentén, egy megadott TraversalOrder szerint (pl. SzélességiElőször, MélységiElőször vagy SkálaNövekvő)  
getStatistics()	Visszaad egy FNDSStatistics struktúrát, amely tartalmazza a gyorsítótár találati arányát, memóriahasználatot és az átlagos fa mélységet  

Gyorsítótárazás és Teljesítmény  
A gyakran hozzáfért fraktális minták magas sebességű elérésének fenntartása érdekében a rendszer egy LRUCache-t valósít meg.

Gyorsítótár Logika: A gyorsítótár FractalNodeData mutatókat tárol. Egy lekérési kérésnél, ha a csomópont megtalálható, akkor a lista elejére kerül („találat”). Ha nem található meg, az FNDSStatistics rögzíti a „nem talált” esetet.  
Statisztikák Nyomon Követése: Az FNDSStatistics struktúra nyomon követi a cache_hit_ratio és total_patterns_indexed értékeket, hogy az ESSO Optimalizáló (lásd 7.3 szakasz) be tudja állítani a fraktál mélységét az optimális következtetési teljesítmény érdekében.  

Integráció az NSIR-rel  
Az FNDS biztosítja a strukturális alapot az Önhasonló Relációs Gráfnak az nsir_core.zig fájlban. Míg az NSIR kezeli a kvantumállapotot és az élek minőségét (szuperpozíció, összefonódás), az FNDS a csomópontok fizikai szervezését kezeli, amikor azok önhasonló mintákat mutatnak a tudásgráf különböző felbontásain belül.
