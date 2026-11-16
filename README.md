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

For the Classical Chebyshev Proof, AI used the Threshold Shrinkage method to prove it. The proof begins with defining Y= X - mean to center the random variable around its mean so that E[Y]= 0 and E[Y2]= Var(X). The proof then defines the tailSign function, denoted by S to determine whether Y would exceed the given threshold t in absolute value and whether it falls in the positive tail or negative tail. When Yt, S=1; when Y-t, S=-1; otherwise, S=0. Through Threshold Shrinkage, the extreme values that exceed the threshold are moved  toward 0  by a distance of t, resulting in the adjusted value Y-tS and reducing their squared deviation from zero. In order to establish the relationship between the original squared deviation and tail probability, AI decomposed Y2 into three terms: 
Y2=(Y-tS)2+t2S2+2t(YS-tS2)

The first term represents the squared deviation after shrinkage, the second term is related to the probability of exceeding the threshold, and the third term represents the remaining nonnegative part of the decomposition. Since both the first term and third term are nonnegative, we can drop these two terms, which gives us:
E[Y^2] >= t^2E[S^2]

Finally, since E[Y2]=Var(X),  and E[S2]=P(|Y|t), we can convert this inequality into:
P(|X-|t)Var(X)t2o

This completes the proof of the classical Chebyshev’s inequality.


To be clear, there was no variation of Catelli’s interval in this representation, and our chatbot did make sure to note that, but rather the final export of the proof of Chebeychev's inequality was formatted so that Catelli’s interval could be invoked if the prior assumptions were met.





