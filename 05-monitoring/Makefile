run:
	uv run python assistant.py

chat:
	uv run streamlit run app.py

network:
	docker network create monitoring

postgres: network
	docker run -it \
		--name course-assistant-pg \
		--network monitoring \
		-e POSTGRES_USER=user \
		-e POSTGRES_PASSWORD=password \
		-e POSTGRES_DB=course_assistant \
		-p 5432:5432 \
		-v pgdata:/var/lib/postgresql/data \
		postgres:17

clear:
	docker stop course-assistant-pg
	docker rm course-assistant-pg
	docker stop grafana
	docker rm grafana
	docker network rm monitoring

dbcheck:
	docker exec -it \
		course-assistant-pg psql \
		-U user \
		-d course_assistant \
    	-c "SELECT id, question, response_time, cost FROM conversations;"

query:
	uv run python db_query.py

dashboard:
	uv run streamlit run dashboard.py --server.port 8502

grafana:
	docker run -it \
		--name grafana \
		--network monitoring \
		-p 3000:3000 \
		-v grafana_data:/var/lib/grafana \
		grafana/grafana