# The design for `infinity`

The key design choice for infinity is to initially have a CLI based interface for projects.

```
# login via an API key
infinity login

# initiate your project in the folder
infinity init ${projectName}
```

You can activate and deactivate logging your results with

```
# activate logging of results
infinity activate

# deactivate logging of results
infinity deactivate
```

---

---

## The `infinity` C design plan

The key thoughts for `infinity` are:

- We want to ensure that the logging doesn't interfere with the timing
- You can name and time any discrete portion of the code.
- All the data is synced online after runs to track the performance progress overtime.
- We can do scaling experiments with ease.
- Native support for multi-threaded code. In particular, tracking all of the above over different numbers of cores.
- You can sweeps across multiple variables.

We achieve this through the idea of _run_'s. A _run_ occurs everytime you run a timing experiment. Run #'s increase linearly overtime and are the key variables used to track time. Each run tracks a number of variables and ensures that

---

### My initial plan is:

- The folder has a `.infinity` folder that contains information for every run.
- Each run is tracked using a .csv file which contains all the information tracked for that run.
- Each run tracks:
  - The git-hash of the current run / Possibly just saves the whole code directory?
  - The user
  - The timing log.
  - The config and the key variables that were tracked during the run.
  - The system hardware of what it was run on.
- Each run has some inherent experiments:

  - The experiment can be simple:

    ```
    // Track a single variable for this run.
    void infinity_log( int key_variable_number, float value)

    // This can be called once per key_variable per run. If called multiple times, only the last time is recorded.
    ```

  - The experiment can scaling-experiment:

    ```
    // Track a variable that will be scaled over a base variable.
    void infinity_scaling_log( int scaling_variable_number, float value)

    // This can be called multiple times per run. If called $n$ times, this will plot these in web app.
    ```

### Current problems:

- How to push data in bulk in a way such that you can annotate certain runs with a string. This is useful if you want to claim
  what those runs are related to.

---

## The `infinity` C++ design plan.

This part should be much easier given a lot of the C++ features. I am focusing on making sure we have a good C interface and then I will add a python version and a C++ version.
