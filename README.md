Floq database replication
=========================

Copies the contents from the production database to test every night so that we
have “real” data during development.

To build the container, run:

    TAG="prod-$(git rev-parse --short HEAD)"
    docker build . -t eu.gcr.io/marine-cycle-97212/floq-db-replicate:prod
    docker tag eu.gcr.io/marine-cycle-97212/floq-db-replicate:prod \
      eu.gcr.io/marine-cycle-97212/floq-db-replicate:${TAG}

To push the container to the registry:

    gcloud docker -- push eu.gcr.io/marine-cycle-97212/floq-db-replicate:prod
    gcloud docker -- push eu.gcr.io/marine-cycle-97212/floq-db-replicate:${TAG}

To update the image used in the cluster:

    kubectl set image deployment/floq-db-replicate floq-db-replicate=eu.gcr.io/marine-cycle-97212/floq-db-replicate:${TAG}
