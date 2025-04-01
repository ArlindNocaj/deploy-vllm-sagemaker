FROM vllm/vllm-openai:v0.7.3

# # Make server compatible with SageMaker Hosting contract
RUN sed -i 's|/v1/models|/ping|g' /usr/local/lib/python3.12/dist-packages/vllm/entrypoints/openai/api_server.py
RUN sed -i 's|/v1/chat/completions|/invocations|g' /usr/local/lib/python3.12/dist-packages/vllm/entrypoints/openai/api_server.py
RUN sed -i 's|/v1/completions|/invocations/completions|g' /usr/local/lib/python3.12/dist-packages/vllm/entrypoints/openai/api_server.py

COPY sagemaker-entrypoint.sh entrypoint.sh
RUN chmod +x entrypoint.sh

ENTRYPOINT ["./entrypoint.sh"]
HEALTHCHECK --interval=30s --timeout=10s --start-period=120s --retries=3 \
  CMD curl -f http://localhost:$PORT/ping || exit 1
