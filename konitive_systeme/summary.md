$\DeclareMathOperator{\sinc}{sinc}$
# Kognitive Systeme

## Signalverarbeitung
Teil der Perzeption. Filtern unwichtiger Information um die Datenrate zu reduzieren.

### Fourier
Die Fourierreihe stellt trigonometrische Funktionen dar:

$f(t)=\frac{a_0}{2}+\sum_{n=1}^\infty a_n \cos(\omega n t)+b_n \sin (\omega n t)$ mit $\omega = \frac{2\pi}{T}$

Die entsprechende komplexe Darstellung ergibt sich zu:

$f(t)=\sum_{n=-\infty}^\infty c_n e^{in\omega t}$ mit $c_n= 
	\begin{cases}
		\frac{1}{2}a_0, & \text{for } n=0\\
        \frac{1}{2}(a_n-ib_n), & \text{for } n>0\\
        \frac{1}{2}(a_{-n}+ib_{-n}), & \text{for } n<0
    \end{cases}$

**Transformation und Rücktransformation**:

$F(\omega)=\int_{-\infty}^\infty f(t)e^{-i\omega t}dt$
$f(t)=\frac{1}{2\pi}F(\omega)e^{i\omega t}d\omega$

Dabei bezeichnet $F(\omega)$ das Spektrum, $F|(\omega)|$ das Amplitudenspektrum und $\phi(\omega)$ das Phasenspektrum.  

Neben dem kontinuierlichen (nicht-periodischen) Fall können auch zeitdiskrete Reihen (ggf. nicht-periodisch) mittels Fourier in ein kontrinuierliches Spektrum tranformiert werden ($\Rightarrow$ *Diskrete Zeit Fouriertransformation*). Diskrete, periodische Reihen können in diskrete komplexe Zahlen transformiert werden ($\Rightarrow$ *Diskrete Fouriertransformation* (DFT)). Die DFT kann als Spezialfall der Z-Transformation angesehen werden.

**Fast Fourier Transformation**:
Wird genutzt um diskrete Fourier-Transformationen schneller durchzuführe. Divide-and-Conquer-Schema: Unterteile Reihe mit Länge $2n, n \in \mathbb{N}$ in gerade und ungerade Indizes, bestimme deren DFT und führe die Teilergebnisse zusammen. 
Laufzeit von $O(n\cdot log(n))$.

### Faltung
$(f \star g)(t)=\int_{-\infty}^\infty f(t-\tau)g(\tau)d\tau$ bzw. im zeitdiskreten Raum $(f \star g)[i]=\sum_{j=-\infty}^{\infty}f[i-j]g[j]$.

Rechenregeln:

  * $F(f_1(t)\star f_2(t))=F_1(\omega)\cdot F_2(\omega)$
  * $F(f_1(t)\cdot f_2(t))=F_1(\omega) \star F_2(\omega)$

### Digitalisierung 

**Dirac-Stoß**:
Fläche eines Dirac-Stoßes beträgt 1, der eigentliche Stoß ist jedoch beliebig schmal und hoch. Es gilt die sogenannte *Siebeigenschaft*:

$\int_{-\infty}^\infty \delta (x)\cdot g(x)dx = g(0)$
$\int_{-\infty}^\infty \delta(x-\tau)\cdot g(x)dx = g(\tau)$

#### Sampling 
Kontinuierlich $\Rightarrow$ diskret. Zeitdiskret. Idealerweise dargestellt durch Multiplikation mit einem Impulszug/Dirac-Kamm.

**Nyquist-Shannon Theorem**:
Für ein kontinuierliches, bandbegrenztes Signal mit Grenzfrequenz $F$ muss mit einer Frequenz größer als $2F$ abgetastet werden um das ursprüngliche Signal rekonstruieren zu können. 
Eine Bandbegrenzung ist mittels Tiefpassfilter erreichbar.

**Aliasing**:
Wenn das Nyquist-Shannon Theorem nicht eingehalten wird, kommt es zu Aliasing durch Überlappung der einzelnen Frequenzen. 
	
**Kurzzeitspektralanalyse**:
Spektrum einer ganzen Aufnahme nicht hilfreich, sondern nur Freqnzenzzusammensetzung innerhalb bestimmten Zeitraums.
Dazu kann die DFT für aperiodische Eingaben genutzt werden, in dem nur ein Zeitfenster betrachtet und als periodisch angenommen wird. Problem: Es gibt einen unauflösaren Widerspruch zwischen Zeit- und Frequenzauflösung. 

Da eine Hutfunktion transformiert eine $\sinc$-Funktion ergibt, ist die Fensterung hiermit nicht ideal, da sie das wahre Kurzzeitspektrum des Signals überlagern würde $\Rightarrow$ es sollte eine andere Fensterfunktion verwendet werden.

#### Quantisierung
Analog $\Rightarrow$ digital. Wertdiskret. 
Quantisierungsstufen bestimmen Auflösung. Dynamische Abtastung ermöglichen passgenauere Auflösung in Abhängigkeit der Signalübergänge.  

Durchschnittlicher Quantisierungsfehler: $e_{avg}[i]=\frac{f_{max}-f_{min}}{2n}$

## Klassifikation/Supervised
Supervised Training wird genutzt wenn es vorgegebene Klassen gibt, welche vorab bekannt sind. Unsupervised Training wird bei unbekannten Klassen/Features angewendet.

**Korrelation**:
Zusammenhang zwischen Signalen (Nächster Wert vom vorigen Wert des anderen Signals). Autokorrelation beschreibt Zusammenhang eines Signals mit sich selbst.


**Template Matching**:
Maß der Übereinstimmung zwischen Schablone und Muster soll maximiert werden $\Leftarrow$ Korrelation in Abhängigkeit des Zentrierungspunktes bestimmen.


### Parametrische Modelle
Parametrische Modelle nehmen eine zugrundeliegende Wahrscheinlichkeitsverteilung an. 

**Bayes Decision Theorie**:

$P(\omega_j | x)=\frac{P(x|\omega_j) P(\omega_j)}{P(x)}$ mit $P(x)=\sum_j P(x|\omega_j)P(\omega_j)$
Dabei ist $P(\omega_j)$ die A-priori-Wahrscheinlichkeit und $P(\omega_j|x)$ die A-posteriori-Wahrschienlichkeit nach Observation von $x$. $P(x|\omega_j$ nennt sich klassenabhängige Wahrscheinlichkeitsdichte.

Probleme: Limitierte Trainingsdaten und Rechenpower, Labeling potenziell fehlerbehaftet und teuer. Klassen können vorab unbekannt sein, keine guten Features bekannt. 
Bei einer parametrischen Lösung kann man Annehmen dass $P(x|\omega_i)$ eine bestimmte parametrische Form hat, meist wird hierfür eine Normalverteilung gewählt.$

**Gaussian Classifier**:

Univariate Normalverteilung: $P(x)=\frac{1}{\sqrt{2\pi \omega}}e^{-\frac{(x-\mu)^2}{2\sigma^2}} \sim N(\mu, \sigma^2)$

Bei multivariaten Normalverteilungen müssen die Parameter für jede Klasse separat geschätzt werden, beispielsweise mittels Maximum Likelihood Estimation.


### Nicht-parametrische Modelle
Generelles Problem bei parametrischen Modellen: Normalverteilung modelliert die Daten eventuell nicht gut. Andere Modelle aber mathematisch schwierig zu lösen. Dann Verwendung nicht-parametrischer Modelle! Auch bei unsupervised Learning anwendbar.

Bei unsupervised Training wird der Error direkt anhand der Daten ermittelt.

**Parzen Windows**:

$P(x) \cdot V =\frac{k}{n}$ mit $k$ als Anzahl Samples im Fenster und $n$ der Anzahl Samples ingesamt und $V$ der Anzahl Samples im Fenster.
Für große $n$ lässt sich so die Wahrscheinlichkeitsdichte ohne zugrundeliegende Verteilungsannahmen berechnen.

**K-Nearest Neighbors**:

Ein Punkt wird der am meisten vorkommenden Klasse unter den $k$ nächsten Samples zugeordnet.

Normalerweise wird $k=\sqrt n$ gesetzt. Grundsätzliches Problem: $k$ sollte groß genug sein, um zuverlässige Werte zu liefern, aber klein genug um ausreichende Nähe der Nachbarn zu garantieren. 

**Curse of Dimensionality und Principal Component Analysis**:
I.A. führen mehr Features zu schlechteren Ergebnissen aufgrund limitierter Datenverfügbarkeit $\Rightarrow$ Features sinnvoll wählen, nur so viele wie nötig.

PCA reduziert Dimensionalität. Grundlegende Annahme ist existierende Korrelation zwischen Dimensionen.

  1. Finde Achse mit größter Datenvarianz
  2. Rotiere den Raum um diese Achse um Korrelation zwischen Dimensionen zu eliminieren
  3. Entferne niedervariante Dimensionen (kaum Informationsverlust)

**Risiko**:

Verlustfunktion: $\sigma(\alpha_i | \omega_j)$ ist der Verlust durch Action $\alpha_i$ im Zustand $\omega_j$.

Der erwartete Verlust bei gegebenem $x$ beträgt dann $R(\alpha_i | x)=\sum_{j=1}^s \lambda (\alpha_i | \omega_j) P(\omega_j | x)$.

**Minimum Error Rate Classification**:
Minimiere Errorrate $\lambda(\alpha_i | \omega_j) = 1 :\Leftrightarrow i\ne j$

**Linear Discriminant Functions**:
Supervised: $g(x)=w_0 + \sum_{i=1}^n w_ix_i$ mit $x_0 = 1$ $\Rightarrow$ Finde Seperator welcher Klassen gut teilt. 

In Abhängigkeit der Relation zu $g_l(x)$ wird dann jedem Punkt eine Klasse zugeordnet. Der Wert kann als Abstand zur Decision Surface interpretiert werden.

**Fisher-Linear Discriminant**:
Set von mehrdimensionalen Punkten wird auf eine Gerade $y$ projiziert. Dann wird die Trenngerade so gewählt, dass der Mittelwert beider Klassen, geteilt durch die Summe der Standardabweichungen der Klassen maximal ist.

### Clustering/Unsupervised

**Mixture Gaussians**:
Bestimme über unsupervised Training Parameter und Gewichtung von mehreren Gauß-Verteilungen die zusammen die Daten abbilden. 

**Hierarchical Clustering/KNN**:
Beginne mit einem Cluster pro Punkt. Merge zwei Cluster die am nächsten gelegenen sind bis gestoppt wird. Der Prozess kann in einem Dendrogram dargestellt werden, welche die Ähnlichkeit innerhalb eines Clusters zeigt.

Maße für Ähnlichkeit:

  * Minimaler Abstand
  * Durchschnittlicher Abstand
  * Maximaler Abstand

### Neuronale Netze
Nicht-parametrisch.

**Perzeptron**:
Gewichtete Summe der Inputs evaluieren. Entweder wird eine Klasse zugewiesen ($>0$?) oder mittels Hard Limiter/ Threshold Logic/ Sigmoid-Funktion ein Output ausgegeben.

Die Funktion $J_p(\vec{w})=\sum_{\vec{x}\in X}(-\vec{w}\cdot \vec{x})$ gibt für die Klasse der fehlklassifizierten Punkte $X$ den Error an. Dieser sollte minimiert werden (Gradientensuchverfahren). Als Error kann der absolute oder quadrierte Abstand verwendet werden.

**Layers**
Eingabeschicht, Ausgabeschicht und ggf. versteckte Schichten.

Ausdrucksfähigkeit:

  * Keine versteckte Schicht: lineare Probleme
  * Eine versteckte Schicht: XOR und 2-dimensional leicht clusterbare Probleme
  * Probleme die sich nicht bildlich clustern lassen

Output eines NN repräsentiert a posteriori Wahrscheinlichkeiten. Error wird idR mittels MSE berechnet.

**Spezifikationen**: Netztopologie, Lernfunktion, Zielfunktion, Eingangsgewichte, Lernparameter

**Performance**
Gründe für schlechte Performance/Generalisierung:

  * Overfitting durch zu viele Iterationen/Lernzyklen $\Rightarrow$ Validation Data nutzen
  * Zu viele Parameter/Gewichte
  * Unpassende Netz-Architektur


## Spracherkennung 
Analoge Sprache $\Rightarrow$ erkannte Sequenz $\Rightarrow$ beste Wortsequenz.

**Word Error Rate**:
Methode um die Qualität der Spracherkennung zu messen.

$WER=\frac{\text{#Ins}+\text{#Del}+\text{#Sub}}{n}$


### Vorverarbeitung
**Alignment**:
Problem: Welche Teile sind einzelne Worte/Aussagen? Menschen sprechen unterschiedlich schnell. Passiert dies innerhalb derselben Äußerung benötigt man nicht-lineares Alignment. 

**Dynamic Time Warping**:
Matrix, wie bei Editing Distance nutzt ein Distanzmaß um Hypothese und Referenz zu vergleichen.

Um die Suche zu vereinfachen, kann man *Beam-Search* verwenden. Dabei werden nur die Zustände mit einer ausreichend geringen kummulierten Distanz weiter expandiert um die Suchzeit zu verringern.

### Akustisches Modell

#### Markow-Ketten
Beschreiben stochastische Prozesse. Für Spracherkennung werden zeitdiskrete Modelle 1. Ordnung (also nur abhängig vom direkt vorhergehenden Zustand) mit endlichem Zustandsraum verwendet.
Zudem sollten die Ketten homogen sein, d.h. Übergangswahrscheinlichkeiten zwischen Zuständen sind unabhöngig von der Zeit.

Topologie:

  * Ergodisch: voll vermascht
  * Left-to-right: Übergänge nur zu Zuständen mit höherem Index und zu sich selbst. Dadurch wird eine zeitliche Ordnung enforct.

**Nicht-beobachtbare Zustände/HMMs**:

Probleme:

  * Evaluierung: Gegeben HMM und Ausgabe, finde Wahrscheinlichkeit mit der die Ausgabe von diesem HMM stammt. Forward oder Viterbi.
  * Dekodierung: Gegeben HMM, finde Zustandsfolge mit höchster Wahrscheinlichkeit. Viterbi (sucht nur max. Wahrscheinlichkeit).
  * Training: Finde gegeben einem HMM bessere Parameter. Forward-Backward.

Es ist kein Verfahren bekannt, um Topologie simultan zuden restlichen Parametern zu optimieren. 

**Kontext**:
Da Markow-Ketten keinen Kontext miteinbeziehen, ist es sinnvoll hybride aus HMMs und NN zu schaffen. 

#### Wörterbücher
Phonetische oder Wörterbücher, ggf. Tree-structured.

#### Sprachmodell
**Fundamentalformel**:
Statistische Methode zur Erkennung von gesprochener Sprache.

$\hat{W}=\arg \max_{w} P(W | X) = \arg \max_{w} \frac{P(X|W)\cdot P(W)}{P(X)}$ 

Hierbei ist $\hat{W}$ die Hypothese, $X$ der Merkmalsvektor, $P(X|W)$ das akustische Modell und $P(W)$ das Sprachmodell. $w$ ist das Wörterbuch (?). 

*Rechenhinweis*: Der Nenner kann weggelassen werden, wenn es nur um einen Vergleich von zwei Modellen geht. 

