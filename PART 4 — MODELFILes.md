You already saw them in README, but GitHub requires separate files.

Example:

📄 setup/model_profiles/qwen-optimal/Modelfile

FROM qwen2.5-coder:1.5b

SYSTEM """
Code-first.
Minimal explanation only if requested.
"""

PARAMETER temperature 0.15
PARAMETER top_p 0.85
PARAMETER repeat_penalty 1.2
PARAMETER num_ctx 1024


(Repeat for the others.)

