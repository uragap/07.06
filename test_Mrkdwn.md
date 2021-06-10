The metrics that make up Core Web Vitals will [evolve](#evolving-web-vitals)
over time. The current set for 2020 focuses on three aspects of the user
experience—_loading_, _interactivity_, and _visual stability_—and includes the
following metrics (and their respective thresholds):

<div class="w-stack w-stack--center w-stack--md">
  <img src="lcp_ux.svg" width="400px" height="350px"
       alt="Largest Contentful Paint threshold recommendations">
  <img src="fid_ux.svg" width="400px" height="350px"
       alt="First Input Delay threshold recommendations">
  <img src="cls_ux.svg" width="400px" height="350px"
       alt="Cumulative Layout Shift threshold recommendations">
</div>

For each of the above metrics, to ensure you're hitting the recommended target
for most of your users, a good threshold to measure is the **75th percentile**
of page loads, segmented across mobile and desktop devices.
