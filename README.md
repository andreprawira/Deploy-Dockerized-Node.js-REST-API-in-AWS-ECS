1. Create 3 different repo registries with 3 different names:
- users
- posts
- threads

2. Launch an ECS cluster using the Cloudformation template:


```aws cloudformation deploy --template-file infrastructure/ecs.yml --region us-east-2 --stack-name ECS-Cluster --capabilities CAPABILITY_NAMED_IAM```
```aws cloudformation deploy --template-file infrastructure/ecs.yml --region <region> --stack-name <stack name> --capabilities CAPABILITY_NAMED_IAM```


3. Create image out of each local folder then push it to ECR

- cd to users folder then ```docker build -t users .```
- cd to posts folder then ```docker build -t posts .```
- cd to threads folder then ```docker build -t threads .```

4. Create tasks definition for each image, all default settings except use port ```3000 in Container port``` for all containers and ```256 Hard memory limits``` 

5. Create service definition for each task, all default settings except ```2 number of tasks``` for each service definition and use Application Load Balancer with ```80:HTTP in Production listener port*```

Path Pattern	Evaluation order Health check path
/api/users*	1			 /api/users
/api/posts*	2 			 /api/posts
/api/threads* 	3			 /api/threads

6. Run all service and task definitions, go to Load Balancer and copy the DNS name, open Postman paste it there
```<DNS name>/api/users```
```<DNS name>/api/threads```
```<DNS name>/api/posts```

ex:```http://ecs-c-ecsal-13ulavsslsxyo-1954189.us-east-2.elb.amazonaws.com/api/users```