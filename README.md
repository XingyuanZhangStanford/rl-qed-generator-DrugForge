# Reinforcement Learning for Drug-like Molecule Generation

This repository contains the implementation and experiments for a reinforcement learning framework designed to generate drug-like molecules represented as SMILES strings. The objective is to optimize the QED (Quantitative Estimate of Drug-likeness) metric while maintaining chemical validity and structural novelty. The project combines a pre-trained LSTM-based SMILES language model, REINFORCE policy-gradient fine-tuning, and Proximal Policy Optimization (PPO) with an actor–critic architecture.

RDKit is used for molecular validation and QED computation, and the GuacaMol dataset serves as the primary training corpus.

De novo molecular design can be formulated as a sequential decision-making problem. A Gym-like environment (SmilesRLEnv) is implemented to support this interaction loop. Reinforcement learning is applied on top of a pre-trained SMILES generator to bias synthesis toward high-QED molecular structures.

A two-layer LSTM language model (embedding dim = 256, hidden size = 512) is trained via maximum-likelihood estimation on 200k molecules sampled from the GuacaMol dataset.
This model serves as a chemically informed prior, capable of generating ~90% valid molecules before reinforcement learning.

The pre-trained model is fine-tuned with the REINFORCE algorithm. The terminal reward is the QED score of the generated molecule. A running reward baseline is used for variance reduction. This method exhibits strong performance and maintains high internal diversity among generated molecules.

An LSTM actor–critic model is trained using Proximal Policy Optimization. Clipped policy updates stabilize training, and a value network provides lower-variance advantage estimates. Although PPO achieves the highest average QED, it tends to collapse to a narrow set of molecular scaffolds, demonstrating a classical mode-collapse phenomenon.

<img width="900" height="614" alt="QED-PPO" src="https://github.com/user-attachments/assets/1f70598a-1d9e-46a7-9a47-2632fbbc9103" />

As result indicated, reinforcement learning substantially improves the drug-likeness (QED) of generated molecules. Both REINFORCE and PPO preserve chemical validity due to the strong SMILES prior. PPO's optimization strength comes at the cost of diversity, collapsing onto a limited scaffold family. REINFORCE maintains broader exploration while achieving nearly comparable QED performance. These observations underscore the exploration–exploitation tradeoff in molecular reinforcement learning and the importance of explicit diversity regularization when using strong policy optimizers such as PPO.

This is a final project for Stanford CS238, Autumn 2025. Full version of project report and discussion could be found in the course website: https://aa228.stanford.edu/old-projects/.
