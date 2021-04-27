---
layout: page
title: Predictive decision support platform 
description: and its application in crowding prediction and passenger information generation
img: /assets/img/tube_crowd.jpeg
importance: 1
category: work
---

Demand for public transport has witnessed a steady growth over the last
decade in many densely populated cities around the world. However, capacity has not
always matched this increased demand. As such, passengers experience long waiting
times and are denied boarding during the peak hours. Crowded platforms and the
subsequent customer dissatisfaction and safety issues have become a serious concern. 
The COVID-19 pandemic has dramatically reduced passengers' willingness to board crowded trains, causing a surge in demand for real-time crowding information. 
In this paper, we propose a real-time predictive decision support platform which addresses both, operations control and customer information needs. The system provides crowding predictions on trains and platforms, communicates this information to passengers, and takes into account their response to it. It is demonstrated through a case study that providing predictive information to passengers can potentially reduce denied boarding and lead to better utilization of train capacity. 


<div class="row justify-content-sm-center">
    <div class="col-l mt-12 ">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/tflVid.gif' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    An interactive tool for visualizing crowding on trains and at stations(<a href="https://github.com/PepSalehi/Network_Crowding"> Github </a>)
</div>

The decision support adopts a rolling-horizon approach. At the start of the updating phase, e.g. 8 am, the system makes predictions for the next 15 (1-step) and 30 (2-steps) minutes based on the available information. 
The predicted OD flows are used to predict future system performance and generate predictive crowding information that is used by passengers to make a decision about which train to board. 
Figure below illustrates the structure of the computational engine of the decision support platform. It consists of two components: the demand engine, and the simulation engine. The demand engine uses historical and real-time AFC data to predict future OD flows.
The simulation model, using as input the train positions at 8 am, simulates the demand and supply interactions for the next prediction horizon (e.g., 30 minutes). At the end of this time, it outputs the predicted levels of train and platform crowding.
One of the main goals is to communicate this crowding information to passengers in the real-world, which in turn might affect their trip making decisions. As such, the decision support must also account for the impact of crowding information. 
The simulation leverages the latest information to correct its prediction of the train positions and loads at that time (see section \ref{chp6:selfcorrect} for details). It uses the 1-step and 2-step ahead OD predictions, the latest ones available at 8:15 am, for representing demand, and re-simulates the network as described before. In this way, simulation is always correcting itself based on the real-time information and re-evaluates its predictions of the crowding levels. This is specially important in cases of significant disruptions, where the deviations between the schedule and reality may become significant.

<div class="row justify-content-sm-center">
    <div class="col-l mt-12 ">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/tflVid.gif' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    An interactive tool for visualizing crowding on trains and at stations(<a href="https://github.com/PepSalehi/Network_Crowding"> Github </a>)
</div>

<div class="row justify-content-m-center">
    <div class="col-m-6 mt-3 mt-md-0">
        <img height="400" src="{{ '/assets/img/pred_framework.png' | relative_url }}" alt="" title="example image"/>
    </div>
    <div class="col-m-6 mt-3 mt-md-0">
        <img height="400" src="{{ '/assets/img/sim_flow.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    left: Overview of the predictive decision support platform right: Overview of the simulation flow
</div>


The crowding information needs to be transformed to a format that is easily comprehensible by the passengers. Table below describes one possible color-based encoding of this information.
Figure below shows the proposed information interface that is displayed at station platforms. For each upcoming train, passengers can see its predicted arrival time and space availability. The design allows passengers to decide whether to defer boarding the approaching train if the next that follows provides a better experience. For example, if it is shown that boarding the approaching train is unlikely, passengers know that even if they succeed at boarding the train, it will be at the crushing level. As such, some passengers might decide to wait for the next train, if the displayed information indicates higher comfort levels. 

<div class="row justify-content-s-center">
    <div class="col-m-6 mt-3 mt-md-0">
        <img height="200" src="{{ '/assets/img/information.png' | relative_url }}" alt="" title="example image"/>
    </div>
    <div class="col-m-6 mt-3 mt-md-0">
        <img height="200" src="{{ '/assets/img/info_encoding.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
     Information displayed to passengers waiting on platforms
</div>


Figure shown below compares the predicted number of passengers denied boarding with the actual values, for different probabilities of deferring $p$. $p=0$ represents the base case where no predictive information is available. It can be seen that as $p$ increases, meaning that passengers decisions are more responsive to the information about crowding in the upcoming trains, the number of denied boarding decreases.
%This is expected, since corresponds to the situation that passengers will increasingly try to board the first train, regardless of their comfort levels. 
This is not surprising. When it is communicated to the passengers that boarding the arriving train is unlikely, and they are more responsive to information (i.e. larger $p$ values), many may decide to wait for the next train. Since these passengers have chosen to defer boarding the arriving train, they are not considered as being denied boarding. 
It also shows the number of passengers who decided to wait for the next train, as a function of the probability $p$. As passenger decisions become more influenced by the information on upcoming trains, the number of deferred boardings increases.
<div class="row justify-content-sm-center">
    <div class="col-l mt-12 ">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/impact.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    Impact of information as measured by deferring probability
</div>

A salient feature of the decision support platform is providing information on expected available space on upcoming trains. Based on this information, some passengers might skip boarding the current train, and wait for the next one. However, if the predicted available space on an upcoming train differs significantly from the observed space upon the train arrival, it can lead to passenger distrust of such information. 

The following compares the predicted and actual train crowding information, for different levels of passengers' responsiveness to information (i.e. $p$), with gradient boosted trees (left) and the historical average (right) used for predicting OD flows. The overwhelming number of trains have enough space to board all the passengers waiting on a platform (i.e. color-coded as Green). Passengers are expected to be most agitated when the upcoming train was communicated to provide enough space for all to board, and yet there was limited space to board upon arrival. This corresponds to a predicted color of Green, while the actual one is Red.
In contrast, it shows the accuracy of the predicted trains' crowding (relative to the actual values), when the historical average is used for OD prediction, for the peak of the peak periods. The number of trains which were wrongly predicted to have enough capacity to guarantee successful boarding significantly increases. Inaccurate information may lead to erosion of trust, can affect public perception of the transit reliability, and highlights the importance of accurate OD predictions.
<div class="row justify-content-sm-center">
    <div class="col-l mt-12 ">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/trust.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    Predicted vs observed train boarding colors
</div>

