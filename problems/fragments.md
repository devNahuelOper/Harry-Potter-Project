Practice your query syntax using GraphiQL to get a feel for how schemas are set up in GraphQL. For problems that accept variables make sure to test each answer you come up with with multiple `id`s to make sure they work.

1. Write a query that will return the name, founder, ghost, and animal of Gryffindor (id: 1) and Ravenclaw (id: 4). Remember to alias - then DRY up your query with a fragment!

fragment findHouse on House {
  name
  founder
  ghost
  animal
}

query NameForTwoHouses($firstId: Int, $secondId: Int) {
  firstHouse: house(id: $firstId) {
    ...findHouse
  },
  secondHouse: house(id: $secondId) {
    ...findHouse
  }
}

2. Write a query that will return the core, length, owner name, and the name of the owner's house for both the wand with the `id` of 7 and the wand with the `id` of 10. Then use a fragment to DRY it up!

fragment findWand on Wand {
  core
  length
  wizard {
    name
    house {
      name
    }
  }
}

query getWandProps($firstId: Int, $secondId: Int) {
  firstWand: wand(id: $firstId) {
    ...findWand
  },
  secondWand: wand(id: $secondId) {
    ...findWand
  }
}

3. Create a query that will accept an `id` variable and will return the patronus form associated with that `id`.

query fetchPatronusForm($patronusId: Int) {
  patronus(id: $patronusId) {
    form
  }
}

4. Write a query with the operation name of `FetchWizardandWand` that will accept two variables, one for a wizard to be fetched(`$wizardId`) and one for a wand to be fetched(`$wandId`). For the wizard return their name and house name. For the wand return the core, length, and the wizard's patronus form.

query FetchWizardandWand($wizardId: Int, $wandId: Int) {
  wizard(id: $wizardId) {
    name
    house {
      name
    }
  },
  wand(id: $wandId) {
    core
    length
    wizard {
      patronus {
        form
      }
    }
  }
}

5. Now let's use variables, aliases and fragments! Write a query that will accept the `id` of two patronus. For each patronus return the form of that patronus, along with the name of the wizard that patronus belongs to.

fragment patronusDetails on Patronus {
  form
  wizards {
    name
  }
}


query fetchPatronuses($patOne: Int, $patTwo: Int) {
  firstPatronus: patronus(id: $patOne) {
    ...patronusDetails
  }
  secondPatronus: patronus(id: $patTwo) {
    ...patronusDetails
  }
}

6. Write a query that accepts two variables for the `id`s of two houses. For each house return the names of all the wizards of that house along with the core of their wands and their patronus forms. Use a fragment!

fragment houseDetails on House {
  name
  wizards {
    name
    wands {
      core
    }
    patronus {
      form
    }
  }
}

query fetchHouses($houseOne: Int, $houseTwo: Int) {
  firstHouse: house(id: $houseOne) {
    ...houseDetails
  }
  secondHouse: house(id: $houseTwo) {
    ...houseDetails
  }
}

7. Write a query that accepts three variables for the `id` for three separate wizards. For the first wizard return their name, house name and patronus form. For the second wizard return their name, their house name, and their wand core. For the third wizard return their name, their house name, their patronus form, and their wand core. Though you are returning different information for each wizard you are still returning the name and house name of each wizard meaning you could use a fragment to DRY this up!

fragment findWizard on Wizard {
  name
  house {
    name
  },
  
}

query ThreeWizards($wizOne: Int, $wizTwo: Int, $wizThree: Int) {
  firstWizard: wizard(id: $wizOne) {
    ...findWizard,
    patronus {
    	form
  	}
  },
  secondWizard: wizard(id: $wizTwo) {
    ...findWizard,
    wands {
      core
    }
  },
  thirdWizard: wizard(id: $wizThree) {
    ...findWizard,
    patronus {
    	form
  	},
  	wands {
    	core
  	}
  }
}

8. Write a query that will accept three variables for a query that can be broken down into three parts. The first variable will be the `id` for a house where you will return the name, founder, and patronus forms of all the wizards in that house. The second variable will be to fetch the length of a particular wand. The third variable will query to find the name, and the patronus form for the wizard with the specified id.

fragment wizardPatronus on Wizard {
  name
  patronus {
    form
  }
}

query MixedQuery($houseId: Int, $wandId: Int, $wizardId: Int) {
  house(id: $houseId) {
    name
    founder
    wizards {
     ...wizardPatronus
    }
  },
  wand(id: $wandId) {
    length
  },
  wizard(id: $wizardId) {
   ...wizardPatronus
  }
}