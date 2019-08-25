#### Smallest Sufficient Team

> In a project, you have a list of required skills req_skills, and a list of people.  The `i`-th person `people[i]` contains a list of skills that person has.
>
> Consider a sufficient team: a set of people such that for every required `skill` in `req_skills`, there is at least one person in the team who has that skill.  We can represent these teams by the index of each person: for example, `team = [0, 1, 3]` represents the people with skills `people[0], people[1], and people[3]`.
>
> Return any sufficient team of the smallest possible size, represented by the index of each person.
>
> You may return the answer in any order.  It is guaranteed an answer exists.
>
> Example 1:
> ```
> Input: req_skills = ["java","nodejs","reactjs"], people = [["java"],["nodejs"],["nodejs","reactjs"]]
> Output: [0,2]
> ```
> Example 2:
> ```
> Input: req_skills = ["algorithms","math","java","reactjs","csharp","aws"], people = [["algorithms","math","java"],["algorithms","math","reactjs"],["java","csharp","aws"],["reactjs","csharp"],["csharp","math"],["aws","java"]]
> Output: [1,2]
> ```
> 
> Constraints:
> 
> *  `1 <= req_skills.length <= 16`
> *  `1 <= people.length <= 60`
> *  `1 <= people[i].length, req_skills[i].length, people[i][j].length <= 16`
> *  Elements of `req_skills` and `people[i]` are (respectively) distinct.
> *  `req_skills[i][j]`, `people[i][j][k]` are lowercase English letters.
> *  Every skill in `people[i]` is a skill in `req_skills`.
> *  It is guaranteed a sufficient team exists.

##### Solution

This is a set covering problem, which is NP-complete. This means that we really can't do better than brute force. 

We'll rely on backtracking to generate all combinations and try to construct the smallest team possible, but we can skill use some optimizations to speed up our search:

1. Convert all `people` entries to sets. This makes processing easier since we can use intersections and unions to add/remove skills.
2. If `people[i]` is a subset of `people[j]`, set `people[i]` to be an empty set. We don't want to remove it because it messes up the ordering, and we need to return the indices of the people in the smallest team.
3. Construct a dictionary of skills to people - for each skill, map it to a set of all people with that skill. The trick here is that `len(people) <= 60`, whereas `len(req_skills) <= 16`, so it makes sense to loop through `req_skills` instead of `people`.

```py
class Solution:
    def smallestSufficientTeam(self, req_skills: List[str], people: List[List[str]]) -> List[int]:
        
        # Convert skills to set for easier processing
        for i, skills in enumerate(people):
            people[i] = set(skills)
            
        # Remove all skill sets that are subset of another skillset, by replacing the subset with an
        # empty set. We do this rather than completely removing, so that indexes aren't 
        # disrupted (which is a pain to have to sort out later).
        for i, i_skills in enumerate(people):
            for j, j_skills in enumerate(people):
                if i != j and i_skills.issubset(j_skills):
                    people[i] = set()
        
        # Now build up a dictionary of skills to the people who can perform them. The backtracking algorithm
        # will use this.
        skills_to_people = collections.defaultdict(set)
        for i, skills in enumerate(people):
            for skill in skills:
                skills_to_people[skill].add(i)
        
        # Keep track of some data used by the backtracking algorithm.
        self.unmet_skills = set(req_skills) # Backtracking will remove and readd skills here as needed.
        self.smallest_length = math.inf # Smallest team length so far.
        self.current_team = [] # Current team members.
        self.best_team = [] # Best team we've found, i,e, shortest team that covers skills
        
        # Here is the backtracking algorithm.
        def meet_skill(skill_idx=0):
            # Base case: all skills met
            if not self.unmet_skills:
                # If the current team is smaller than the previous we found, update it.
                if len(self.current_team) < self.smallest_length:
                    self.best_team = self.current_team[::]
                    self.smallest_length = len(self.best_team)
                return

            # If this skill is already met, move onto the next one.
            if req_skills[skill_idx] not in self.unmet_skills:
                return meet_skill(skill_idx+1)
                # Note return is just to stop rest of code here running. Return values
				# are not caught and used.
                
            # Otherwise, consider all who could meet the current skill.
            for person in skills_to_people[req_skills[skill_idx]]:
                # Add this person onto the team by updating the backtrading data.
                skills_added_by_person = people[person].intersection(self.unmet_skills)
                self.unmet_skills -= skills_added_by_person
                self.current_team.append(person)
                
                # Do the recursive call to further build the team.
                meet_skill(skill_idx + 1)
                
                # Backtrack by removing the person from the team again.
                self.current_team.pop()
                self.unmet_skills = self.unmet_skills.union(skills_added_by_person)
    
        meet_skill(0)
        return self.best_team
```
