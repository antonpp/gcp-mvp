## Deploying Flask App with CI/CD and Workstation

### 1. Setting up Google Cloud

1. Navigate to [console.cloud.google.com](https://console.cloud.google.com) and create an account.
2. Select a project or click **NEW PROJECT** to create `my-mvp` project.
3. Head to **cloud workstations(*)**.
   - Enable the API.
   - Navigate to **clusters** > create cluster and select `europe-west1`. Note: This process takes approximately 15 minutes.

(*)Cloud Workstations are a way to start developing. With this service you get an instance with pre-installed and pre-configured VS Code and other tools like gcloud. Another convenient option is to use [Cloud Shell](https://cloud.google.com/shell), just make sure to commit your work to Github when you're done. Or you can ofcourse still use your laptop too, but in that case you'll need to [install gcloud yourself](https://cloud.google.com/sdk/docs/install). If you do feel free to skip this section.

### 2. Setting up GitHub

1. While the cluster is being created, head to [GitHub](https://github.com/) and create an account if you haven't.
2. Create a new empty private repository named `/my-demo-app`. Ensure to add `.gitignore` and `README`.
3. Take a break! Maybe grab a coffee for 12 minutes (that's the time it generally takes for the cluster to be created).

### 3. Setting up Cloud Workstations

1. Return to **cloud workstations**.
2. Navigate to **Configurations** > create configuration > configure & create.
3. Go to **My Workstations** > create.
4. Start the workstation once it's done provisioning and launch it when it's running.

### 4. Working within the Workstation

1. Upon entering the workstation, close any welcome screen and mark tasks as "done".
2. Clone your Git Repository. Allow the GitHub extension to sign in and finish the authentication flow.
3. Clone the repository created in the previous section, `my-demo-app`, and open the folder once cloned.

### 5. Deploying a Python Service to Cloud Run

We will now follow the steps from the [Deploy a Python service to Cloud Run](https://cloud.google.com/run/docs/quickstarts/build-and-deploy/deploy-python-service) tutorial. See [here](https://cloud.google.com/run/docs/quickstarts) for other languages. If you'd like to deploy a LLM langchain app on Google Cloud see [here](https://github.com/GoogleCloudPlatform/generative-ai/tree/main/language/orchestration/langchain) and [here](https://cloud.google.com/blog/products/ai-machine-learning/generative-ai-applications-with-vertex-ai-palm-2-models-and-langchain).

1. Within the workstation, open a new terminal and enter:
   ```bash
   gcloud init
   ```
   Follow the on-screen instructions.
2. Set your project by running:
   ```bash
   gcloud config set project PROJECT_ID
   ```
3. Create the files `main.py`, `requirements.txt`, `Dockerfile`, and `.dockerignore` as instructed in the [tutorial](https://cloud.google.com/run/docs/quickstarts/build-and-deploy/deploy-python-service).
4. Deploy to Cloud Run with:
   ```bash
   gcloud run deploy
   ```
   and follow the instructions. Note: The Cloud Run container will be deployed under the `run.app` domain.

### 6. Setting up Continuous Deployment

1. Head to **Cloud Run** in the console and select the deployed app `my-demo-app`.
2. Click **set up continuous deployment**.
3. If prompted, click **enable cloud build api**.
4. For Repository Provider, choose **GitHub**.
5. Authenticate with GitHub when prompted and select the repo you created earlier.
6. Complete the **Build Configuration** and select the Dockerfile. Save your settings.

### 7. Observing the Build Process

1. You should now observe a 'Building and deploying from repository' stage. Click on **logs** to check on the build progress.

### 8. Testing CI/CD

1. Go to **Cloud Build** > **History**.
2. Head back to **Cloud Workstation** and make a modification in `main.py`.
3. Configure git's email and name in the Workstation terminal:
   ```bash
   git config --global user.name "John Doe"
   git config --global user.email johndoe@example.com
   ```
4. Stage, commit, and push the changes. For instance:
   ```bash
   git add .
   git commit -m "initial commit"
   git push main
   ```
5. Navigate back to **Cloud Build history**. You should see a new build starting.
6. To verify the changes, navigate to the Cloud Run URL once it's done deploying.
