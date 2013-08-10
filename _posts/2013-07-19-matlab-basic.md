---
layout: post
title: "python learning"
category: posts
---


abc:
$$\alpha2 = \frac{1}{y2} ( \zeta-\alpha_1y_1 ) \overset{y_2=-1,1}{=} y_2\left(\zeta-\alpha_1y_1\right)$$

ddd

##Feature normalization

    function [X_norm, mu, sigma] = featureNormalize(X)
    % X is a matrix
    mu = mean(X)
    sigma = std(X)

    X_norm = bsxfun(@minus, X, mu)
    X_norm = bsxfun(@rdivide, Xnorm, sigma)
