# <div align="center">💫 Welcome to my digital playground! <img src="https://raw.githubusercontent.com/MartinHeinz/MartinHeinz/master/wave.gif" width="35px"></div>

<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=12,2,20,24&height=170&section=header&text=Felipe%20Adeildo&desc=Building%20from%20scratch,%20one%20line%20at%20a%20time&fontSize=35&descSize=20&fontAlignY=25&descAlignY=45&animation=fadeIn"/>
</div>

<div align="center">
  <a href="https://felipeadeildo.com">
    <img src="https://img.shields.io/badge/Portfolio-felipeadeildo.com-FF6B6B?style=for-the-badge&logo=firefox&logoColor=white"/>
  </a>
  <img src="https://komarev.com/ghpvc/?username=felipeadeildo&style=for-the-badge&color=FF6B6B"/>
</div>

## About Me 🎭

```python
from typing import List, Literal, Optional, TypedDict, final
from enum import Enum, auto


class Interest(Enum):
    MATHEMATICS = auto()
    NEURAL_NETWORKS = auto()
    WEB_SCRAPING = auto()


class ProjectInfo(TypedDict):
    name: str
    description: str
    tech_stack: List[str]


@final  # This class should not be inherited
class Developer:
    def __init__(
        self,
        name: str,
        role: str,
        interests: List[Interest],
        favorite_anime: Optional[str] = None,
        favorite_character: Optional[str] = None
    ) -> None:
        """Initialize a Developer with strong typing and validation.
        
        Args:
            name: The developer's full name
            role: Current professional role
            interests: List of validated interests from Interest enum
            favorite_anime: Optional favorite anime
            favorite_character: Optional favorite character
        """
        self.__name: str = name
        self.__role: str = role
        self.__interests: List[Interest] = self.__validate_interests(interests)
        self.__favorite_anime: Optional[str] = favorite_anime
        self.__favorite_character: Optional[str] = favorite_character
        self.__philosophy: str = "Building everything from scratch"
    
    @property
    def name(self) -> str:
        return self.__name
    
    @staticmethod
    def __validate_interests(interests: List[Interest]) -> List[Interest]:
        """Validate that all interests are from the Interest enum."""
        if not all(isinstance(interest, Interest) for interest in interests):
            raise ValueError("All interests must be from the Interest enum")
        return interests
    
    def get_current_focus(self) -> ProjectInfo:
        """Get information about current focus project."""
        return {
            "name": "Neural Network Implementation",
            "description": "Developing neural networks from scratch using pure NumPy",
            "tech_stack": ["Python", "NumPy"]
        }
    
    def get_programming_style(self) -> Literal["from scratch", "with frameworks"]:
        """Get the developer's programming philosophy."""
        return "from scratch"  # We always choose the hard way 💪
    
    def get_expertise_level(self, tech: str) -> Literal["beginner", "intermediate", "expert"]:
        """Get expertise level for a specific technology."""
        # Implementation would vary based on tech
        return "expert" if tech.lower() in ["python", "numpy"] else "intermediate"


# Usage example:
me = Developer(
    name="Felipe Adeildo",
    role="Computer Science Student",
    interests=[Interest.MATHEMATICS, Interest.NEURAL_NETWORKS, Interest.WEB_SCRAPING],
    favorite_anime="Hunter x Hunter",
    favorite_character="Hisoka 🃏"
)
```

## My Arsenal 🎯

<div align="center">

![Python](https://img.shields.io/badge/Python-14354C?style=for-the-badge&logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![NumPy](https://img.shields.io/badge/Numpy-777BB4?style=for-the-badge&logo=numpy&logoColor=white)
![HTTPX](https://img.shields.io/badge/HTTPX-242A2D?style=for-the-badge&logo=python&logoColor=white)

</div>

## Highlighted Projects 💎

<div align="center">

[![Neural Network Card](https://github-readme-stats.vercel.app/api/pin/?username=felipeadeildo&repo=neural-network&theme=radical)](https://github.com/felipeadeildo/neural-network)
[![Cantina CF Card](https://github-readme-stats.vercel.app/api/pin/?username=felipeadeildo&repo=cantinacf&theme=radical)](https://github.com/felipeadeildo/cantinacf)
[![QueryTube Card](https://github-readme-stats.vercel.app/api/pin/?username=felipeadeildo&repo=querytube&theme=radical)](https://github.com/felipeadeildo/querytube)

</div>

## GitHub Magic ✨

<div align="center">
  <img height="180em" src="https://github-readme-stats.vercel.app/api?username=felipeadeildo&show_icons=true&theme=radical&include_all_commits=true&count_private=true"/>
  <img height="180em" src="https://github-readme-stats.vercel.app/api/top-langs/?username=felipeadeildo&layout=compact&langs_count=7&theme=radical"/>
</div>

## Let's Connect! 🌐

<div align="center">

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/felipeadeildo)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:contato@felipeadeildo.com)
[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/felipeadeildo)

</div>

<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=12,2,20,24&height=100&section=footer"/>
</div>

<!--START_SECTION:waka-->
<!--END_SECTION:waka-->
