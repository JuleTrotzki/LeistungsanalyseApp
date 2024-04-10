# LeistungsanalyseApp

##main.py:
from my_functions import estimate_max_hr, build_person, build_experiment
import json

def main():
    experiment_name = input("Enter experiment name: ")
    date = input("Enter date of experiment (YYYY-MM-DD): ")
    supervisor_first_name = input("Enter supervisor's first name: ")
    supervisor_last_name = input("Enter supervisor's last name: ")
    supervisor_sex = input("Enter supervisor's sex (male/female): ")
    supervisor_age = int(input("Enter supervisor's age: "))
    subject_first_name = input("Enter subject's first name: ")
    subject_last_name = input("Enter subject's last name: ")
    subject_sex = input("Enter subject's sex (male/female): ")
    subject_age = int(input("Enter subject's age: "))

    supervisor = build_person(supervisor_first_name, supervisor_last_name, supervisor_sex, supervisor_age)
    subject = build_person(subject_first_name, subject_last_name, subject_sex, subject_age)

    experiment = build_experiment(experiment_name, date, supervisor, subject)

    print("Experiment details:")
    print(experiment)

    with open("experiment.json", "w") as outfile:
        json.dump(experiment, outfile, indent=4)

if __name__ == "__main__":
    main()

##my_functions.py:
def estimate_max_hr(age_years : int , sex : str) -> int:
  """
  See https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4124545/  [Titel anhand dieser PMC-ID in Citavi-Projekt übernehmen]  for different formulas
  """
  if sex == "male":
    max_hr_bpm =  223 - 0.9 * age_years
  elif sex == "female":
    max_hr_bpm = 226 - 1.0 *  age_years
  else:
    # der input() öffnet ein Eingabefenster für den Nutzer und speichert die Eingabe
    max_hr_bpm  = input("Enter maximum heart rate:")
  return int(max_hr_bpm)

def build_person(first_name, last_name, sex, age) -> dict:
    """Returns a dictionary of information about a supervisor or subject."""
    dict = { "first_name" : first_name,
             "last_name" : last_name,
             "age" : age,
             "estimate_max_hr" : estimate_max_hr(age,sex)}
    return dict

def build_experiment(experiment_name, date, supervisor, subject) -> dict:
    """Returns a dictionary of information about an experiment."""
    dict = {"experiment_name" : experiment_name,
            "date" : date,
            "supervisor" :   supervisor,
            "subject" :   subject
            }
    return dict
