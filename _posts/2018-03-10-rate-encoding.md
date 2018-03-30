---
layout: post
category : neural nets
title: Rate Encoded Location
tagline: place encode like a brain
tags : [place encoding, rate encoding]
description: An outline of the basic idea about rate encoded locations, why it is important, how it might be done in parts of the brain, and how to approximate the same behavior in an artificial neural network.
authors: [jeblad]
sources: [ ["This link does not work", "https://github.com/jeblad/brain/some-place/some-place-deep.pdf"], ["This link does not work", "https://github.com/jeblad/brain/some-place/some-other-place/"] ]
use_math: true
paper_url: https://github.com/jeblad/brain/some-place/some-place-deep.pdf
paper_title: Title of the article
repo_url: https://github.com/jeblad/brain/some-place
repo_title: Deep into the repository
license: cc-by-sa
---
{% include JB/setup %}

It is known from [grid cells](https://en.wikipedia.org/wiki/grid_cells) that the place can be encoded as an $N$-tuple of neuronal activities, where the activity of each neuron approximate a cosine. This set encodes the location over several orders, and forms a reference grid for recall of position. We can hypothesize that something similar happen in other neural networks, but we don't really know for sure.

In the neocortex there are structures called [cortical columns](https://en.wikipedia.org/wiki/cortical_column) and [cortical minicolumns](https://en.wikipedia.org/wiki/cortical_minicolumn), or cortical macro and micro columns. The general idea is that a cortical column is a set of features that somehow are grouped together, and that a cortical minicolumn form a specific kind of feature extractor. One interpretation is that as a minicolumn gets activated, and as long as the extracted feature stays inside the [receptive field](https://en.wikipedia.org/wiki/receptive field) of the minicolumn it stays active. It will although try to rate encode the location inside that receptive field, going from low to high over some interval.

If we assume there is a kind of reference grid overlaid the receptive field, then the actual input can be added to the set of neuronal activities in this reference grid. If the neurons in the reference grid has [burst firing](https://en.wikipedia.org/wiki/burst_firing) modus operandi, and the neuron is viewed as a leaky bag, then the input together with the bursts will give rise to a [pulse position modulated](https://en.wikipedia.org/wiki/Pulse-position_modulation) spike train. This happen if the input value arrive rather sparingly compared to the grid values, and the neuron thus has time to integrate over all grid values. The input signal can then be viewed as a set value, and the neuron slowly integrates the grid values ramping them up until they reach threshold value, and then a spike is fired. The outgoing spike can then be observed as a pulse position encoded location.

Let the positional inputs be $p_i(t)$ with weights $w_i$, and the signal inputs be $x_j(t)$ with weights $v_j$, then the output $y_j$ will be

$$
y_j = \operatorname{q} \left ( \delta_{j-1}, v_j \int_t^{t+\Delta t} x_j(t)\, dt + \sum_i \left ( w_i \int_t^{t+\Delta t} p_i(t)\, dt \right ) \right )
$$

where $\operatorname{q}$ is some spiking transferfunction, that somehow maintain a previous state $\delta_{j-1}$. This can also be modeled as differential equations.

The spike will arrive sooner when the signal is high and later when the signal is low. Likewise, it will arise sooner when the grid is high and later when the grid is low. At the same time the spike can be assumed to reset the cycle, even if there are no input signal the grid will still generate a background spike chatter. Still note that this is not a strictly linear representation, it is a good bit of randomness in the spiking behavior.

By inspecting the equation it is obvious that if the inputs are continuous, or close to continuous, and there are no kind of external reference, then the observed pulse position modulation turns into a kind of plain rate encoding. Typically this rate encoded spiking is what would be observed from the axon.

If a minicolumn consists of about 100 neurons, and they are divided over about six layers, then there are about 16-17 neurons in each layer. That gives approximate four times four neurons in the input layer, or about four neurons to form a two times two grid with sufficient resolution. If the encoding isn't completly binary, then the number of neurons in the grid could be even fewer. That could indicate that the actual neurons that has this role would be rather few and interspersed with other neurons.

It is also plausible that the receptive field could be somewhat larger than the minicolumn itself. That is even more plausible if the column does [principal component analysis](https://en.wikipedia.org/wiki/principal_component_analysis) or [independent component analysis](https://en.wikipedia.org/wiki/principal_component_analysis).
