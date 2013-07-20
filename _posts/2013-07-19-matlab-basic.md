---
layout: post
title: "python learning"
category: posts
---

##Feature normalization

    function [X_norm, mu, sigma] = featureNormalize(X)
    % X is a matrix
    mu = mean(X)
    sigma = std(X)

    X_norm = bsxfun(@minus, X, mu)
    X_norm = bsxfun(@rdivide, Xnorm, sigma)
