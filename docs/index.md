### Min Specialisering  
Jeg har valgt at specialisere mig inden for backend-udvikling og Blazor-webapplikationer. Jeg arbejder på et projekt for Odense Golfklub, hvor fokus er at udvikle en løsning, der giver golftræneren bedre indsigt i sine spillere og deres præstationer. Projektet automatiserer manuelle opgaver som statistikanalyser, hvilket sparer tid og øger effektiviteten. Gennem projektet har jeg arbejdet med principper som Clean Architecture og CQRS i backend, samtidig med at jeg har skabt en dynamisk og brugervenlig frontend i Blazor, der integrerer med en sikker backend via API'er. I frontend har jeg især arbejdet med interaktive funktioner som søgning, sortering og farvekodning af data for at skabe en mere visuel og brugervenlig oplevelse.

---

### Backend

#### Viden  
- Jeg har viden om:
  - Principperne bag **Clean Architecture**, hvor ansvarsområder opdeles i lag som `Domain`, `Application`, `Infrastructure` og `Presentation (API)`.
  - Hvordan **Repository-pattern** bruges til at håndtere datatilgang og abstraktion.
  - Hvordan **CQRS-pattern** adskiller kommandoer og forespørgsler for bedre modularitet.
  - Sikkerhedsmekanismer som **JWT-tokens** til autentifikation.
  - Hvordan validering implementeres i backend ved hjælp af FluentValidation.

#### Færdigheder  
- Jeg kan:
  - Implementere **Clean Architecture** ved at strukturere backend i adskilte lag.
  - Udvikle løsninger baseret på **Repository-pattern** til CRUD-operationer.
  - Anvende **CQRS-pattern** til at adskille læse- og skriveoperationer.
  - Implementere sikkerhed ved hjælp af **JWT-tokens**.
  - Sikre dataintegritet ved hjælp af FluentValidation.

#### Kompetencer  
- Jeg kan:
  - Evaluere behovet for designmønstre som **Repository-pattern** og **CQRS** i forskellige projekter.
  - Kombinere principperne for **Clean Architecture** med designmønstre for at skabe en robust og vedligeholdelsesvenlig backend-løsning.
  - Reflektere over valget af sikkerhedsmekanismer og tilpasse dem til projektkrav.

---

### Blazor

#### Viden  
- Jeg har viden om:
  - Hvordan **Blazor WebAssembly** bruges til at udvikle moderne, komponentbaserede webapplikationer.
  - Principperne for **state management**, der sikrer dynamisk opdatering af brugergrænsefladen.
  - Hvordan API-integration understøtter kommunikationen mellem frontend og backend.
  - Visualisering af data ved hjælp af farvekodning baseret på beregnede værdier.

#### Færdigheder  
- Jeg kan:
  - Dynamisk opdatere UI ved hjælp af **state management** i Blazor.
  - Integrere frontend og backend ved hjælp af API-kald via `ApiService`.
  - Skabe interaktive komponenter, der anvender databinding, events, søgning og sortering.
  - Implementere dynamisk farvekodning af data for at fremhæve nøgleområder.

#### Kompetencer  
- Jeg kan:
  - Designe og implementere avancerede datavisualiseringer og interaktive komponenter.
    - Eksempel: Søgefunktion, tabelsortering og dynamisk farvekodning i `Stats.razor`.
  - Kombinere API-integration og UI-design for at skabe en dynamisk og brugervenlig komponent.
  - Iterativt forbedre brugergrænsefladen baseret på feedback og brugerinteraktioner.

---


### **Logbog**

#### **13. august – 10. oktober: Projektopstart og backend-design**

**Indledning**  
Projektet startede med en analyse af kravene fra Odense Golfklub. Målet var at designe en fleksibel backend-struktur baseret på **Clean Architecture**, som kunne håndtere spillerdata og statistik. Denne fase inkluderede også planlægning af integrationen mellem backend og frontend via API’er.

**Læringsmål og fokusområder**  
- Forstå og implementere principperne bag **Clean Architecture**.
- Opdele backend i klare lag for at sikre vedligeholdelse og testbarhed.
- Identificere systemkrav gennem møder med Product Owner og domæneanalyse.

**Struktur af Clean Architecture**:
```plaintext
GolfCoachInsight
│
├── Domain
│   └── Entities
│
├── Application
│   ├── Features
│   ├── Validators
│   ├── Contracts
│   └── Services
│
├── Infrastructure
│   ├── Persistence
│   │   └── Repositories
│
└── API
    └── Controllers
```
![Dependency flow](img/CleanA.png)


**Refleksion over læring**  
Ved at implementere Clean Architecture lærte jeg, hvordan lagdeling kan give fleksibilitet og understøtte fremtidig udvidelse. For eksempel gjorde det det nemt at tilføje nye funktioner som statistikberegninger uden at påvirke de eksisterende kernefunktioner.

**Resultater og effekter**  
- En solid arkitektur blev etableret, som sikrer skalerbarhed og fleksibilitet.
- Designbeslutninger blev dokumenteret, hvilket gjorde det lettere at forklare projektet til teamet og interessenter.

---

#### **11. oktober – 5. november: Backend-udvikling og kernefunktionaliteter**

**Indledning**  
Fokus var på at implementere backend-funktionaliteter ved brug af **CQRS-pattern**, **FluentValidation** og **JWT-tokens**. Det sikrede en struktureret og sikker backend.

**Læringsmål og fokusområder**  
- Implementere CQRS for at adskille kommandoer og forespørgsler.
- Validering af inputdata med FluentValidation.
- Sikring af API’et med JWT-tokens for autentifikation.

**Eksempler fra koden:**  
CQRS Handler:
```csharp
public class GetGolfClubListRequestHandler : IRequestHandler<GetGolfClubListRequest, List<GolfClubDto>>
{
    private readonly IGolfClubRepository _golfClubRepository;

    public GetGolfClubListRequestHandler(IGolfClubRepository golfClubRepository)
    {
        _golfClubRepository = golfClubRepository;
    }

    public async Task<List<GolfClubDto>> Handle(GetGolfClubListRequest request, CancellationToken cancellationToken)
    {
        var golfClubs = await _golfClubRepository.GetAllAsync();
        return golfClubs.Select(gc => new GolfClubDto
        {
            Id = gc.Id,
            Name = gc.Name,
            Location = gc.Location
        }).ToList();
    }
}
```

Validering med FluentValidation:
```csharp
public class CreateGolfClubDtoValidator : AbstractValidator<CreateGolfClubDto>
{
    public CreateGolfClubDtoValidator()
    {
        RuleFor(g => g.Name).NotEmpty().WithMessage("Golf club name is required.");
        RuleFor(g => g.Email).EmailAddress().WithMessage("A valid email is required.");
    }
}
```

**Refleksion over læring**  
Arbejdet med CQRS lærte mig at tænke på adskillelse af ansvar og modularitet. JWT-tokens gav mig praktisk erfaring med at balancere sikkerhed og brugervenlighed. En stor udfordring var at debugge token-relaterede fejl, hvilket styrkede mine færdigheder i fejlfinding.

**Resultater og effekter**  
- Robust backend, der kan håndtere komplekse forespørgsler og kommandoer.
- Sikker datatilgang og forbedret brugeroplevelse gennem validering og autentifikation.

---

#### **6. november – 25. november: Frontend-udvikling**

**Indledning**  
I denne fase arbejdede jeg med at udvikle en dynamisk brugergrænseflade i **Blazor WebAssembly**, som understøtter interaktiv funktionalitet og integration med backend.

**Læringsmål og fokusområder**  
- Skabe interaktive og brugervenlige komponenter.
- Integrere frontend og backend effektivt via API’er.
- Implementere funktionalitet som dynamisk filtrering og statistikvisning.

**Eksempler fra koden:**  

Stats-tabel med farvekodning:
```razor
<td class="outlined" style="background-color:@GetColor(stat.FairwayHitsPercentage, minFairwayHits, maxFairwayHits)">
    @Math.Round(stat.FairwayHitsPercentage, 2)
</td>
```

Golf Club Oversigt:
```razor
@foreach (var club in golfClubs)
{
    <a @onclick="() => SelectClub(club.Id)" class="list-group-item list-group-item-action">
        @club.Name
    </a>
}
```

**Refleksion over læring**  
Farvekodning og dynamiske filtre blev udviklet iterativt med feedback fra PO. Funktionen `GetColor` forbedrede visualiseringen og blev rost som værende intuitiv under tredje feedbackmøde.

**Resultater og effekter**  
- Intuitiv frontend med dynamisk funktionalitet.
- Effektiv kommunikation mellem frontend og backend.

---