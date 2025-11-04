<h1>Methods × Design Decisions</h1>

<table>
  <tr>
    <th style="min-width:170px;">Method (paper)</th>
    <th style="min-width:300px;">How the steering vector is obtained</th>
    <th style="min-width:200px;">Where to intervene</th>
    <th style="min-width:150px;">Granularity</th>
    <th style="min-width:180px;">Static vs. input-dependent</th>
    <th style="min-width:150px;">What tokens are steered</th>
    <th style="min-width:200px;">Strength / normalization & tuning</th>
    <th style="min-width:250px;">Main goal / effects</th>
  </tr>

  <tr>
    <td><b>Inference-Time Intervention (ITI)</b></td>
    <td>Train linear probes per attention head; use probe normal or pos–neg mean-difference; select top-K heads</td>
    <td>Add to output of selected attention heads</td>
    <td><b>Head-wise sparse</b> (top-K)</td>
    <td><b>Static</b> per objective</td>
    <td>Language tokens (LLMs)</td>
    <td>Scale by α×σ along direction; tune K, α</td>
    <td>Improves TruthfulQA; modest OOD gains; minimal ability degradation</td>
  </tr>

  <tr>
    <td><b>Textual Steering Vectors for MLLMs</b></td>
    <td>Build concept vectors from text-only models (SAE / probe / mean-shift); inject to MLLM; grid-search layer & α</td>
    <td>Add to hidden at chosen middle layer</td>
    <td><b>Token-set level</b> (image/text/both)</td>
    <td><b>Static</b> per skill</td>
    <td>Image + text tokens</td>
    <td>Grid search layer ℓ and α; different ranges for normalized vs unnormalized</td>
    <td>Improves counting & spatial reasoning</td>
  </tr>

  <tr>
    <td><b>Learn-to-Steer (L2S)</b></td>
    <td>Start with contrastive prompting (P2S) to get per-input vectors; train auxiliary net to predict vector</td>
    <td>Residual pathway</td>
    <td><b>Per-input</b></td>
    <td><b>Input-dependent</b> (learned)</td>
    <td>Multimodal tokens</td>
    <td>Network learns when to steer; α selected for utility</td>
    <td>Safety steering; reduces harmfulness but preserves normal answers</td>
  </tr>

  <tr>
    <td><b>GrAInS</b></td>
    <td>Use Integrated Gradients to find top tokens; contrastive deltas; PCA → layer-wise vector</td>
    <td>Add at each steered layer; renormalize</td>
    <td>Token-aware, layer-wise</td>
    <td><b>Input-aware</b> via attribution</td>
    <td>LLM + VLM tokens</td>
    <td>λ strength then L2 renorm</td>
    <td>Improves truthfulness; reduces hallucination; better alignment</td>
  </tr>

  <tr>
    <td><b>Unified Understanding & Evaluation</b></td>
    <td>Unifies CAA / RepE / ITI; shows mean-difference (CAA) is theoretically optimal; formalizes variants</td>
    <td>Discusses extraction positions (attn / MLP / residual)</td>
    <td>Highlights global-vector failure modes</td>
    <td>N/A</td>
    <td>N/A</td>
    <td>Formalizes CAA, PCA, probe, ITI normalization</td>
    <td>CAA > PCA/Classifier; guidance on layer/location & context</td>
  </tr>

  <tr>
    <td><b>MAT-STEER</b> (Multi-Attribute Targeted Steering)</td>
    <td>Learn attribute vectors on pos/neg activations; sparsity + orthogonality; token-level gating</td>
    <td>Add to hidden states at chosen layers</td>
    <td><b>Per-token & per-attribute</b></td>
    <td><b>Input-dependent</b> (gated)</td>
    <td>Language tokens</td>
    <td>Gate-weighted α; reduces attribute conflict</td>
    <td>Jointly improves multiple attributes (truth, toxicity, bias, helpfulness)</td>
  </tr>

  <tr>
    <td><b>ASTRA</b> (Adaptive Steering for VLM Jailbreak Defense)</td>
    <td>Image attribution via random visual-token ablation + Lasso; vector = activation diff</td>
    <td>Layer activations in VLM pipeline</td>
    <td><b>Layer-wise</b>, harmful visual-directions</td>
    <td><b>Input-dependent</b> via projection</td>
    <td>Primary: visual tokens</td>
    <td>Coefficient from projection; single-pass</td>
    <td>Strong jailbreak defense; minimal utility loss; transfers well</td>
  </tr>

  <tr>
    <td><b>CAST</b> (Conditional Activation Steering)</td>
    <td>Learn behavior vector + condition vector(s); apply only if prompt matches condition</td>
    <td>Hidden states at selected layers</td>
    <td><b>Global</b> when triggered</td>
    <td><b>Input-dependent</b> via threshold</td>
    <td>Language tokens</td>
    <td>Apply α·v if similarity > threshold; supports multi-conditions</td>
    <td>Selective refusal; refuses harmful prompts but answers harmless ones</td>
  </tr>

  <tr>
    <td><b>VTI</b> (Visual & Textual Intervention)</td>
    <td>Precompute latent directions approximating averaged perturbed images; optional text intervention</td>
    <td>Vision features + text hidden states</td>
    <td><b>Layer / space-level</b></td>
    <td>Mostly <b>static</b></td>
    <td>Visual + text tokens</td>
    <td>Fixed directions with tunable α</td>
    <td>Reduce hallucinations; improve visual consistency</td>
  </tr>

  <tr>
    <td><b>Modality Preference Steering</b> (MC2)</td>
    <td>Probe reps to find vision–text preference vectors; steer to amplify chosen modality</td>
    <td>Representation space</td>
    <td><b>Global preference</b></td>
    <td><b>Static</b></td>
    <td>Image + text tokens</td>
    <td>Tune α + layer scope</td>
    <td>Control modality preference; improve translation & multimodal understanding</td>
  </tr>

</table>
