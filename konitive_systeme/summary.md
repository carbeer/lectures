# Kognitive Systeme

## Signalverarbeitung
Teil der Perzeption. Filtern unwichtiger Information um die Datenrate zu reduzieren.

### Fourier
Die Fourierreihe stellt trigonometrische Funktionen dar:

$f(t)=\frac{a_0}{2}+\sum_{n=1}^\infty a_n cos(\omega n t)+b_n sin(\omega n t)$ mit $\omega = \frac{2\pi}{T}$

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

Neben dem kontinuierlichen (nicht-periodischen) Fall können auch zeitdiskrete Reihen (ggf. nicht-periodisch) mittels Fourier in ein kontrinuierliches Spektrum tranformiert werden ($\Leftrightarrow$ *Diskrete Zeit Fouriertransformation*). Diskrete, periodische Reihen können in diskrete komplexe Zahlen transformiert werden ($\Leftrightarrow$ *Diskrete Fouriertransformation* (DFT)). Die DFT kann als Spezialfall der Z-Transformation angesehen werden.

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
$\int_{-\infty}^\infty \delta(x-\tau)\cdot g(x)dx = g(\tau)

#### Sampling 
Kontinuierlich $\Leftrightarrow$ diskret. Zeitdiskret. Idealerweise dargestellt durch Multiplikation mit einem Impulszug/Dirac-Kamm.

**Nyquist-Shannon Theorem**:
Für ein kontinuierliches, bandbegrenztes Signal mit Grenzfrequenz $F$ muss mit einer Frequenz größer als $2F$ abgetastet werden um das ursprüngliche Signal rekonstruieren zu können. 
Eine Bandbegrenzung ist mittels Tiefpassfilter erreichbar.

**Aliasing**:
Wenn das Nyquist-Shannon Theorem nicht eingehalten wird, kommt es zu Aliasing durch Überlappung der einzelnen Frequenzen. 
	
**Kurzzeitspektralanalyse**:
Spektrum einer ganzen Aufnahme nicht hilfreich, sondern nur Freqnzenzzusammensetzung innerhalb bestimmten Zeitraums.
Dazu kann die DFT für aperiodische Eingaben genutzt werden, in dem nur ein Zeitfenster betrachtet und als periodisch angenommen wird. 

Da eine Hutfunktion transformiert eine $sinc$-Funktion ergibt, ist die Fensterung hiermit nicht ideal, da sie das wahre Kurzzeitspektrum des Signals überlagern würde $\Leftrightarrow$ es sollte eine andere Fensterfunktion verwendet werden.

#### Quantisierung
Analog $\Leftrightarrow$ digital. Wertdiskret. 
Quantisierungsstufen bestimmen Auflösung. Dynamische Abtastung ermöglichen passgenauere Auflösung in Abhängigkeit der Signalübergänge.  

Durchschnittlicher Quantisierungsfehler: $e_{avg}[i]=\frac{f_{max}-f_{min}}{2n}$




