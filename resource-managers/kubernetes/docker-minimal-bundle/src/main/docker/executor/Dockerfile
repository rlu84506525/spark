#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

FROM openjdk:8-alpine

# If this docker file is being used in the context of building your images from a Spark distribution, the docker build
# command should be invoked from the top level directory of the Spark distribution. E.g.:
# docker build -t spark-executor:latest -f dockerfiles/executor/Dockerfile .

RUN apk upgrade --update
RUN apk add --update bash
RUN mkdir -p /opt/spark
RUN touch /opt/spark/RELEASE

ADD jars /opt/spark/jars
ADD examples /opt/spark/examples
ADD bin /opt/spark/bin
ADD sbin /opt/spark/sbin
ADD conf /opt/spark/conf

ENV SPARK_HOME /opt/spark

WORKDIR /opt/spark

# TODO support spark.executor.extraClassPath
CMD exec ${JAVA_HOME}/bin/java -Dspark.executor.port=$SPARK_EXECUTOR_PORT -Xms$SPARK_EXECUTOR_MEMORY -Xmx$SPARK_EXECUTOR_MEMORY -cp ${SPARK_HOME}/jars/\* org.apache.spark.executor.CoarseGrainedExecutorBackend --driver-url $SPARK_DRIVER_URL --executor-id $SPARK_EXECUTOR_ID --cores $SPARK_EXECUTOR_CORES --app-id $SPARK_APPLICATION_ID --hostname $SPARK_EXECUTOR_POD_IP
