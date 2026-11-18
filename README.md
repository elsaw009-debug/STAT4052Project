Minah Elsawy, Brody Schaffner,Emma Feng, and Elliot Fergle
Chebvychez inequality 

Background Knowledge 


Chebyshev's Inequality is a probabilistic inequality. It provide an upper bound to the probability that the absolute deviation of a random variable from its mean will exceed a given threshold. 

Let X be a random variable while having a finite mean and finite variance. Then, the following inequality, called Chebyshev's Inequality 

P(|X-|t) <=  Var(X)/ t2

Our Goal

The goal of our midterm project was to learn whether an AI model could solve an already existing proof in an unconventional and original way. Choosing a foundational proof in statistics to try this out on Chebyshev's Inequality.

Methods 


Prompted an AI model (ChatGPT), “Can you prove Chebyshev's inequality without Markov's inequality? Find an original way to prove Chebyshev’s inequality. Make sure you look at all available proofs to ensure you do not repeat a proof.” Took a response time of 9 minutes and 21 seconds. 

Lean Code 

The AI Model successfully a lean verified proof. 

FFor the Classical Chebyshev Proof, AI used the Threshold Shrinkage method to prove it. The proof begins with defining Y=X- to center the random variable around its mean so that E[Y]=0 and E[Y2]= Var(X). The proof then defines the tailSign function, denoted by S to determine whether Y would exceed the given threshold t in absolute value and whether it falls in the positive tail or negative tail. When Yt, S=1; when Y-t, S=-1; otherwise, S=0. Through Threshold Shrinkage, the extreme values that exceed the threshold are moved  toward 0  by a distance of t, resulting in the adjusted value Y-tS and reducing their squared deviation from zero. In order to establish the relationship between the original squared deviation and tail probability, AI decomposed Y2 into three terms: 
Y2=(Y-tS)2+t2S2+2t(YS-tS2)

The first term represents the squared deviation after shrinkage, the second term is related to the probability of exceeding the threshold, and the third term represents the remaining nonnegative part of the decomposition. Since both the first term and third term are nonnegative, we can drop these two terms, which gives us:
E[Y2]t2E[S2]

Since S2=1 when |Y|t and 0 otherwise, E[S2] equals the probability that |Y|t.
Finally, since E[Y2]=Var(X),  and E[S2]=P(|Y|t), we can convert this inequality into:
P(|X-|t)Var(X)t2
This completes the proof of the classical Chebyshev’s inequality.
After successfully proving the classical Chebyshev’s inequality, we asked AI further to explore whether the same Threshold Shrinkage method could provide a stronger result. AI found that in the previous proof, we got rid of the first term (Y-tS)2 and the third term 2t(YS-tS2) because both of them are nonnegative. However, in order to get a stricter upper bound of probability, AI further considered the first term  (Y-tS)2. According to the definition of variance, Var(Z)=E[Z]2-(E[Z])^2, and the variance is always nonnegative, therefore E[Z2](E[Z])2. By substituting Z=Y-tS and using the previously established fact that E[Y]=0, we can get: 
E[(Y-tS)2](E[Y-tS])2
                                =(E[Y]-tE[S])2
                               =(0-tE[S])2
                   =t2(E[S])2

This shows that the first term is not just nonnegative, it  can also  provide a better lower bound. Substituting this new lower bound into the previous Y2decomposition formula:
E[Y2]=E[(Y-tS)2]+t2E[S2]+2tE[YS-tS2] 

Since the third term is still nonnegative, we get:
E[Y2]t2(E[S])2+t2E[S2]

Since E[Y2]=Var(X), and E[S2]=P(|X-|t), therefore:
Var(X)t2(E[S])2+t2P(|X-|t)

Subtracting t2(E[S])2 from both sides, and dividing by t2, we get: 
P(|X-|t)Var(X)t2-(E[S])2

Compared to the classical Chebyshev’s inequality, this new bound subtracts an additional term (E[S])2. E[S] represents the difference between the probabilities of the positive and negative tails. When the two probabilities are unequal, (E[S])2is greater than zero, making the new probability bound tighter. If the two probabilities are equal, the bound stays the same.

To be clear, there was no variation of Catelli’s interval in this representation, and our chatbot did make sure to note that, but rather the final export of the proof of Chebeychev's inequality was formatted so that Catelli’s interval could be invoked if the prior assumptions were met.






