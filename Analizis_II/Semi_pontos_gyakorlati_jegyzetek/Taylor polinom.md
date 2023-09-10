## Taylor polinom
+ ![](attachment/d419dd77fb81a8b69670c043093790cd.png)
+ ![](attachment/a9418a7d6abc060bb350e04c95df8341.png)
+ ![](attachment/4d226d41bfeffee4a76a4102dff575a2.png)
+ ![](attachment/95157ab238b3dddd0c960339abfd3937.png)

## Taylor sor
- $\sum _{n=0}^{\infty }\frac{f^{n}\left(x_0\right)\left(x-x_0\right)^n}{n!}$ 
- n=0-nál 
- Maclaurin sor és a taylor sor között annyi a különbség hogy ennél mindig c=0
- f(x)=ln(x) taylor sora c=1 
	- első 4 derivált(a lényeg hogy egy patern kiaalakuljon és a végén legalább 3 nem nulla tag maradjon a sum kialakitáshoz)
		- ![](attachment/11122c00aee2074b79c87e0e7aed7808.png)
	- értéke a fügvénynek és a deriváltaknak c-nél
		- ![](attachment/2fe1a86126a03c1d9a9e618ff5dca75d.png)
	- taylor sor expanded képlet(az x-1 nél nem deriválás van)
		- ![](attachment/19e2cc27823223091ff1ecf312355615.png)
		- 
		- ![](attachment/00f6eee2032f74c3b5f34bc3e5a6d465.png)
	- sum formába átirás
		- ![](attachment/4ce7a55f3cb588fb17f0aadcd0db48dd.png)     
- f(x)=e^x c=3
	- ![](attachment/782b32afa7ec7555d20f56fc4961d2e6.png)
- Maclaurin f(x)=sin(x)
	- ![](attachment/ff6f78af40d8badd04c638014339761f.png)
	- ![](attachment/64848528c52988febe47e3ba5fc7e29e.png)
	- ![](attachment/1c2f71dc0a7829e69544af5c2b181c0f.png)
- cos(x) maclaurin sora sin(x) maclaurin sorával
	- sin(x) macnak mindkét oldalának deriváltja
		- ![](attachment/6507fcab0afe26819d2dd8ff113ea1d7.png)
	- átirni sum formába
		- ![](attachment/f14af425be7e3c783c8d9ddf5bda883a.png)
	- egyszerübb megoldás:
		- ![](attachment/b9f55e7623db8b105b5d143c08dfa9a6.png)
- maclaurin cos(x^2)
	- a cos(x) ismerjük és csak ki kell cserélni a x-et x^2-re
	- ![](attachment/0b9bad4f1e3435ebc5237d0171e13876.png)
- maclaurin xcos(x)
	- ![](attachment/c87d2b2f68046d06549348ba2b4b7d6d.png)
- maclaurin x^2e^(-x)
	- ![](attachment/8519414f6e1dea3168e86e6702398de4.png)
## Maclurian hiba
- Közelités a Leibniz sor újra kezdése elött számolja ki 3 tagig vagyis az integrál értékét
- a hibát a kért 4. tag abszolút értékével adja meg
- ![](attachment/c56c3ee19652ecb45fe6b6da6c7db98c.png)      